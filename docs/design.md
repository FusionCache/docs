---
layout: default
title: Design
nav_order: 2
---

# Design

Fusion prioritises:

- Read query performance (i.e. queries that don't change the cache, such as `GET` and `FIND`)
- Low latency
- Avoid convoluted query interaction
- Avoid endless configuration options



The engine is asynchronous to decouple query execution from query request and responses. The network layer is also asynchronous to avoid threads waiting on network socket operations whilst other tasks can be processed.

<br/>


## Query Interfaces
There are two interface types: REST and WebSocket. There is one REST interface and two WebSocket interfaces:

- WebSocket Normal: for typical query payloads
- WebSocket Bulk: for larger payloads, intended for `STORE` with many objects

Each query has a dedicated buffer. The query is parsed as JSON and then as FQL. If these are successful then the query is passed to the query engine. This is an asychronous operation so the interface thread can continue to serve requests/responses for any client.

When the query execution is complete, the response is sent to the client.

{: .important}
> There is no synchronisation between the REST and WebSocket interfaces. Queries are executed in the order received, which may not be the same when sending to different interfaces.
>
> A client can send queries on different interfaces but only if the order of execution doesn't matter.

<br/>


## Query Engine Overview
The query engine is what actually executes queries:

- A query executes on its own thread
- Each query is completely separate - there is no interthread communication required
- When a read query finishes executing, the response is sent immediately, irrespective of the receive order (explanation further down)

A performance killer for multithreaded software are mutexes used to protect mutable shared data between threads: a thread needs to read or write to a memory location whilst at least one other thread also must read or write to the same location.

If more than one thread can mutate the same memory location, then all threads that access that memory location must use protection, even if the majority of the threads only require read access.


<br/>

## Query Access

Each query type has a data access level:

- read: the query only reads from the cache, does not mutate (`GET`, `FIND`, `COUNT`, etc)
- write: the query mutates the cache (`STORE`, `UPDATE`, `DELETE`, `CREATE_CLASS`, etc)


<br/>

## Query Execution
To reduce data race issues, only one access level can execute at a given time:

- If a write query is executing, no other queries can execute
- If a read query is executing, only read queries can execute


<br/>

### Write Queries

There is one constraint for write queries: there can only be one write query executing.

This may seem a huge disadvantage, but there is an upside: although there can only be one write query executing, it is _**the**_ only query, so no data race protection is required. 



{: .important}
> There is an opportunity to execute multiple write queries in certain circumstances: if write queries are writing to different and unrelated Fusion classes, they can run independently. This will be considered in the future.
>
>"Fusion classes" here refers to the classes you create in the cache, not classes in the Fusion code.


<br/>

### Read Queries

These contraints benefit read queries:
- as a read query executes, there can't be data races because there can't be write queries executing to change the cache
- by definition a read query doesn't change data, so multiple read queries can execute concurrently without data races, therefore no data race protection

If there are no active write queries, a read query must only wait to execute if all threads are busy, otherwise it will always execute immediately and be completely independant from the other read queries (they are all read-only, no data races possible so no interthread communication required).

<br/>

## Query Execution Sequence
With this in mind, the general sequence is:

- Check the query queue:
  - If it is empty then wait
  - Otherwise dequeue a query
- If there are no active queries, execute this query immediately
- If there are active queries, check access levels:
  - If this query is write and the active query is write:
    - Send this query to execute but don't execute until the active write query is complete
  - If this query is read and the active queries are read:
    - Execute this query immediately on the next available thread
  - Otherwise it is an access level mismatch
    - Wait for the active queries to finish then execute this query immediately


<br/>

## Query Response Order


### Write Queries
The gaurantees apply only when sending from the same client on the same query interface:

- Execution: in the order received
- Responses: sent in the order received

### Read Queries

- Execution: in the order received
- Responses: no guarantees, responses are sent immediately when the query finishes

More about that. Let's say a client sends a `FIND` and then a `GET`. A `GET` query involves less work so it'll likely complete first. So even though the `GET` was sent second, it may finish first, therefore its response is sent first.



