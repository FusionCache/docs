---
layout: default
title: Overview
nav_order: 1
has_children: true
---


# Overview

FusionCache is a cache for storing, searching and retrieving JSON data. There are two modes:


**KeyValue**

- Data is handled as key values, similar to Redis or Memcached
- Queries and responses are use a WebSocket interface

<br/>

**Objects**

- Data is handled as JSON objects and relationships between objects are tracked
- Queries and responses are handled by REST and WebSocket interfaces, with a dedicated WebSocket interface for bulk data

<br/>

{: .important}
> From here, FusionCache is referred to as just Fusion.

<br/>


## Start Here
Fusion is available as a Debian package. An Alpine Linux based Docker image will be released soon.

{: .important}
> Fusion is only available for 64bit x86 CPUs. An ARM build will be available in the future.
>
> It has not been tested on Mac or Windows, although it should run on WSL2 in Windows.
>

<br/>

- [Quickstart](guides/quickstart/quickstart.md)
- [Objects](objects.md) and [Concepts](concepts.md)
- [Design](design.md) for details of the internals

<br/>

## Limitations
Fusion is still in alpha and has limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Threads| There is a 64 thread limit. This is not a technical limitation, it will increase as development progresses.|
|Memory| There is no data eviction, nor any protection against memory use. This means Fusion will eat system memory, causing the OS to use swapped memory if the heap is full. <br/> Of course you can delete data at any time.<br/> A future release will address this. |
|Security| The query interfaces use HTTP rather than HTTPS. Fusion is not intended for a public network.<br/>There is no user authentication or a way to assign query types to a user, for example to prevent certain users deleting.



