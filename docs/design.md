---
layout: default
title: Design
nav_order: 2
---

# Design

Fusion prioritises low latency for read queries.


<br/>

![Fusion design](images/design_overview.svg)

The network and query execution are asynchronous to decouple query execution from requests and responses. This avoids threads blocking:

- The interface threads pass queries to the query engine, freeing the interface thread to service other network operations
- When a query completes, the query engine passes the response to the interface, so the query engine thread can execute other queries


<br/>


## Query Interfaces
There are two interface types: REST and WebSocket. There is one REST interface and two WebSocket interfaces:

- WebSocket Normal: for typical query payloads
- WebSocket Bulk: for larger payloads, intended for `STORE` with many objects

Each query has a dedicated buffer. The query is parsed as JSON and then as FQL. If these are successful then the query is passed to the query engine. This is an asychronous operation so the network interface thread can continue to serve requests/responses for any client.

When a query execution completes, the response is sent to the client on a separate thread, allowing the query engine thread to execute other queries.

{: .important}
> There is no synchronisation between the REST and WebSocket interfaces. Queries are executed in the order received, which may not be in the same order as sent when to different interfaces.
>
> It is safe to send queries to different interfaces but only if the order of execution is not important.

<br/>


## Query Engine Overview
The query engine is what actually executes queries:

- A query executes on its own thread
- Queries are executed in isolation - no interthread communication is required
- Queries are executed in batches, grouped by the query access type (explained below)

A performance killer for multithreaded software are mutexes.

When multiple threads access the same memory location and just one _may_ write to that location, access by all threads must be protected, even if most of the threads  require only read access.  


<br/>


## Query Access Type
Each query has a data access type:

- Read: the query reads from the cache, it does not mutate (`GET`, `FIND`, `COUNT`, etc)
- Write: the query may mutate the cache (`STORE`, `UPDATE`, `DELETE`, `CREATE_CLASS`, etc)


<br/>


## Query Execution
To reduce data race issues, only one access type can execute at a given time:

- If a write query is executing, no other queries can execute
- If a read query is executing, only read queries can execute


<br/>


### Write Queries
Write queries have a constraint: there can only be one write query executing.

This may seem a huge disadvantage, but there is an advantage: although there can only be one write query executing, it is _**the**_ only query, so no data race protection is required. 



{: .important}
> There is an opportunity to execute multiple write queries in certain circumstances: if write queries are writing to different and unrelated Fusion classes, they can run independently. This will be considered in the future.
>
>"Fusion classes" here refers to the classes you create in the cache, not classes in the Fusion code.


<br/>


### Read Queries
These contraints benefit read queries:
- as a read query executes, there can't be data races because there isn't write queries executing
- by definition a read query doesn't change data, so multiple read queries can execute concurrently without data races, therefore no data race protection is required

If there are no active write queries, a read query must only wait to execute if all threads are busy, otherwise it will always execute immediately and be completely independant from the other read queries: they are all read-only, no interthread communication required so no data races possible.


<br/>


## Query Response Order
Write and read queries are executed in the order received but the response order differs.

<br/>

### Write Queries
Responses are sent in the same order as received.

<br/>

### Read Queries
Responses are sent immediately, even if there is a read query from the same client still executing that was received first.

For example, a client sends two queries:

- The client sends a `FIND`
- The client sends a `GET` 10 microseconds later
- The `FIND` execution takes 80 microseconds
- The `GET` execution takes 50 microseconds

Even though the `GET` was received second, it completes before the `FIND`, therefore the client will receive the `GET_RSP` first.


<br/>


## CPU Utilisation
A queue is used to ensure queries are executed in the correct order. Fusion assumes it will receive many queries so aims to pop the queries from the queue as soon as possible. This process has two states:

1. **Poll:** poll the query queue
2. **Wait:** wait for a query to be pushed onto the queue

The difference is:
- Polling: the queue is constantly checked for queries
- Waiting: waits in a condition variable until it is notified a query has arrived, which requires a mutex for the condition variable

This approach avoids locking/unlocking the mutex when it is expecting queries imminently.

<br/>


### Sequence
This approach maxes one logical core during the poll period, which is 10 seconds. When it enters wait, this reduces to near zero.

The sequence is:

1. Poll the query queue
2. If a query arrives, extend the polling period
3. Repeat (1) and (2) until no queries arrive during the polling period
4. Enter wait
5. When a query arrives, repeat from (1)

