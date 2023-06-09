---
layout: default
title: Overview
nav_order: 1
has_children: true
---


# Overview

FusionCache is an object cache for storing, searching and retrieving JSON data. Queries and responses are handled by REST and WebSocket interfaces, with a dedicated WebSocket interface for bulk data.

Fusion is designed to prioritise read query performance. The engine is developed in C++ and is fully asynchronous to maximise CPU resources.

{: .important}
> From here onwards, FusionCache is referred to as just Fusion.

<br/>


## Query Interfaces
There are three query interfaces:

- REST
- WebSocket Standard
- WebSocket Bulk

The bulk interface can assign a larger buffer per query which is useful when caching with thousands of objects in a single query. In most cases, queries are much smaller, so the standard interface is used.

<br/>


## Start Here
Fusion is available as a Debian package. An Alpine Linux based Docker image will be released soon.

{: .important}
> Fusion is only available for 64bit x86 CPUs. An ARM build will be available in the future.
>
> It has not been tested on Mac or Windows, although it should run on WSL2 in Windows.
>
> The documentation assumes familiarity with JSON and UUIDs (universally unique identifiers).

<br/>

- [Quickstart](guides/quickstart/quickstart.md)
- [Objects](objects.md) and [Concepts](concepts.md)
- [Design](design.md) for details of the internals

<br/>

## Limitations
Fusion is still in alpha and has limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Threads| There is a 64 thread limit. This is not a technical limitation, it will increase as testing progresses.|
|Memory| There is no data eviction, nor any protection against memory use. This means Fusion will eat system memory, causing the OS to use swapped memory if the heap is full. <br/> Of course you can delete objects at any time.<br/> A future release will address this. |
|Security| The query interfaces use HTTP rather than HTTPS. Fusion is not intended for a public network.<br/>There is no user authentication or a way to assign query types to a user, for example to prevent certain users deleting.



