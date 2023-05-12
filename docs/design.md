---
layout: default
title: Design
nav_order: 2
---

# Design

Fusion prioritises:

- Read query performance (i.e. queries that don't change the cache, such as `GET` and `FIND`)
- Low latency
- Unconvoluted query interaction
  - FQL, the JSON based query language, should be easy to learn
- Avoid too many configurable parameters
  - Users know how they'll use the software, but offering too much configurations leads to complexities and error


The engine is asynchronous to decouple query execution from query request and responses. The network layer is also asynchronous to avoid threads waiting on network socket operations whilst other tasks can be processed.

<br/>


## Query Interfaces
There are two interface types: REST and WebSocket. There is one REST interface and two WebSocket interfaces:

- WebSocket Normal: for typical query payloads
- WebSocket Bulk: for larger payloads, intended for `STORE` with many objects

Each query has a dedicated buffer. The query is parsed as JSON and then as FQL. If these are successful then the query is passed to the query engine. This is an asychronous operation so the interface thread can continue to serve requests/responses for any other client.

When the query execution is complete, the response is sent to the client.


<br/>


## Query Engine Overview
The query engine is what actually executes queries:

- A query executes on its own thread
- Each query is completely separate - there is no interthread communication required
- When a read query has finished executing, the response is sent immediately, irrespective of the receive order (explanation further down)

A performance killer for multithreaded software are mutexes used to protect mutable shared data between threads (thread A wants to update a value whilst thread B wants to read or write the same data): 
- a thread arrives at a mutex, checking if it's available
  - if not, it's pushed to the 'waiting' queue
  - if so, it acquires the mutex
- when a thread completes, it releases the mutex
- the next thread is popped from the 'waiting' queue

This takes time, is likely to invalidate cache lines and the waiting threads must be synchronised. 


<br/>

## Query Access

Each query type has a data access level:

- read: the query only reads from the cache, does not mutate (`GET`, `FIND`, `COUNT`, etc)
- write: the query mutates the cache (`STORE`, `UPDATE`, `DELETE`, `CREATE_CLASS`, etc)


<br/>

## Query Execution
To reduce data race issues only one access level can execute at a given time:

- if a write query is executing, a read query cannot execute
- if a read query is executing, a write query cannot execute


<br/>

### Write Queries

There is one constraint for write queries: there can only be one write query executing.

This may seem a huge disadvantage, but there is an upside: although there can only be one write query executing, it is _**the**_ only query, so no data race protection is required. 



{: .important}
> There is an opportunity to execute multiple write queries in certain circumstances: if write queries are writing to different and unrelated classes, they can run independently. This will be considered in the future.


<br/>

### Read Queries

These contraints benefit read queries:
- as a read query executes, there can't be data races because there can't be write queries executing to change the cache
- by definition a read query doesn't change data, so multiple read queries can execute concurrently without data races, therefore no data race protection

This is why read query performance is notable. If there are no active write queries, a read query must only wait to execute if all threads are busy, otherwise it will always execute immediately and be completely independant from the other executing read queries (they are all read-only, no data races possible so no interthread communication required).

<br/>

## Query Execution Sequence
With this in mind, the general sequence is:

- Check the query queue:
  - If it is empty then wait
  - Otherwise dequeue from the queue
- If there are no active queries, execute this query immediately
- If there are active queries:
  - If this query is write and active queries are write:
    - Send this query to execute but don't execute until the active write queries are complete
  - If this query is read and active queries are read:
    - Execute this query immediately on the next available thread
  - Otherwise, wait for active queries to finish then execute immediately

