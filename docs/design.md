---
layout: default
title: Design
nav_order: 3
has_children: false
---


# KV Design
Key value is much simpler than Object mode:

1. There are no relationships to manage
2. No classes to create/maintain
3. No ObjectIDs to create
4. KV queries require less processing


This allows Fusion to optimise how it handles keys to reduce query latency.

<br/>

## Network I/O
The Websocket API has dedicated thread(s) and is decoupled from the query processing. This means an I/O thread is used only when performing an I/O task or when initially parsing the query before execution.

<br/>

## Query Interfaces
KV Mode offers a WebSocket interface, there is no REST interface available.

<br/>

## Parrallel Execution
Unlike Object Mode, KV Mode is executes with both read and write operations concurrently, allowing very high query throughput.

<br/>

## Constraints
The only constraints are:

- keys must be a string
- keys have a minimal length of 6 characters (this may be configurable in a future release)


<br/>
<br/>
<br/>

# Objects Mode

![Fusion design](images/design_overview.svg)


<br/>


## Query Interfaces
There are three interfaces: one REST and two WebSockets:

- REST: for typical query payloads
- WebSocket Standard: for typical query payloads
- WebSocket Bulk: for larger payloads, intended for `STORE` with many objects

Each query has a dedicated buffer. The query is parsed as JSON and then as FQL. If these are successful then the query is passed to the query engine. This is an asychronous operation so the network interface thread can continue to serve requests/responses for any client.

When a query execution completes, the response is sent to the client on a network interface thread, allowing the query engine thread to execute other queries.

{: .important}
> There is no synchronisation between the REST and WebSocket interfaces. Queries are executed in the order received, which may not be in the same order as sent when sent to different interfaces.
>
> It is safe for a client to send queries to different interfaces but only if the order of execution is not important.

<br/>

## Query Response Order
Write and read queries are executed in the order received but the response order differs.


### Write Queries
Responses are sent in the same order as received.


### Read Queries
Responses are sent immediately, even if there is a read query from the same client still executing that was received first.

For example, a client sends two queries:

- The client sends a `FIND`
- The client sends a `GET` 10 microseconds later
- The `FIND` execution takes 100 microseconds
- The `GET` execution takes 60 microseconds

<br/>

![executors](images/design_executors.svg)


Even though the `GET` was received after `FIND`, it completes before the `FIND`, therefore the client will receive the `GET_RSP` first.
