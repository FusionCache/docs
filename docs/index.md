---
layout: default
title: Overview
nav_order: 1
has_children: true
---


# Overview

FusionCache is an object cache for storing, searching and retrieving data. Queries and responses are handled by REST and WebSocket interfaces, with a dedicated WebSocket interface for bulk data.

The priority is reducing latency, particularly for read queries. A read query is one which does not change data, such `GET` or `FIND`. The opposite to this is a write query which changes data, such as `STORE` or `UPDATE`.

{: .important}

> From here onwards, FusionCache is referred to as just Fusion.

<br/>

## Design

Fusion's engine is designed to prioritise read queries, maxing all cores if required. The engine is fully asynchronous to maximise CPU resources. 

### Query Interfaces
There are three query interfaces:

- REST
- WebSocket Normal
- WebSocket Bulk

The bulk interface can assign a larger buffer per query which is useful when receiving large queries, such as a `STORE` with thousands of objects . In most cases, queries are likely to be much smaller, so the normal interface can be used.

The reasons for this:
- Reduce memory usage by users only using/enabling the bulk interface when required
- It is more efficient to issue a single large `STORE` query than thousands of smaller queries.

<br/>


## Limitations
Fusion is still in alpha and has limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Memory | There is no data eviction, nor any protection against memory use. This means Fusion will always attempt to allocate memory, causing the OS to swapped memory if the heap is full. <br/> Of course you can delete objects at any time.<br/> A future release will provide at least one auto eviction policy. |
|REST/WebSocket Interfaces| There are only two threads to handle these interfaces. The bulk has a dedicated thread, whilst the REST and 'normal' (i.e non-bulk) WebSocket interfaces share a thread.<br/> A future release will allow multiple threads per interface, configurable by the user (e.g. you know that you'll rarely use REST and bulk so they can share a thread).|
|Security| The query interfaces use HTTPS, but there is no user authentication or a way to assign query types to a user, for example to prevent certain users deleting.<br/>A possible solution is to use a combination of simple role based authentication and user tokens.

<br/>


## Install
Fusion is only available as a Docker image, Ubuntu and Alpine packages will be available later.


{: important}
> Fusion is only available for 64bit x86 CPUs.
>
> It has not been tested on Mac OS.


<br/>

## Start Here
Start with understanding Concepts then the Quickstart tutorial.

{: .important}
> The documentation assumes you are familiar with JSON and UUIDs (universally unique identifier).

