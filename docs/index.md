# FusionCache Overview

FusionCache is an object cache for storing, searching and retrieving data. Queries and responses are handled by REST and WebSocket interfaces, with a dedicated WebSocket interface for bulk data.

The priority is reducing latency, particularly for read queries. A read query is one which does not change data, such `GET` or `FIND`. The opposite to this is a write query which changes data, such as `STORE` or `UPDATE`.

{: .important}

From here onwards, FusionCache is referred to as just Fusion.

<br/>

## Design

Fusion's engine is designed to prioritise read queries, maxing all cores if required. The engine is fully asychronous, including the network and query execution to maximise CPU resources. 

Fusion is still alpa software with limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Memory | There is no data eviction, nor any protection against memory usage. This means Fusion will attempt to allocate memory, using paged memory if the heap is full. <br/> A future release will provide at least one auto eviction policy. |
|REST/WebSocket Interfaces| There are only two threads to handle these interfaces. The bulk has a dedicated thread, whilst the REST and 'normal' (i.e non-bulk) WebSocket interfaces share a thread.<br/> A future release will allow multiple threads per interface, configurable by the user (e.g. you know that you'll rarely use REST and bulk so they can share a thread).|
|Security| The HTTP interface use HTTPS, but there is not a user authentication or a way to assign query types to a user, such as to prevent only certain users deleting.<br/>A possible solution to use a combination of simple role based authentication and user tokens.

<br/>


## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
