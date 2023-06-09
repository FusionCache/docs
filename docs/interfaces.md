---
layout: default
title: Interfaces
nav_order: 3
has_children: false
---


# Interfaces
Queries are submitted to REST or WebSocket interfaces. There is one REST interface and two WebSocket interfaces.

Each interface can have its max read buffer size set, with each having a maximum defined by Fusion.

<br/>

## Limits

| Interface   | Default     | Minimum   | Maximum     |
|:---         |:---         |:---       |:---         |
|REST         | 1024 bytes  | 64 bytes  | 4096 bytes  |
|WS Standard  | 1024 bytes  | 64 bytes  | 8192 bytes  |
|WS Bulk      | 8 MB        | 1024 bytes| 1 GB        |

Each query uses a buffer large enough for the query and it is not deallocated until query execution completes. A query is rejected if it exceeds the buffer size.

<br/>

## REST
This is a standard HTTP request/response interface. All queries are accepted on this interface, although it involves establishing a new HTTP connection for each query, it should be avoided if querying frequently.

<br/>


## WebSocket Normal
A standard HTTP connection, upgraded to a WebSocket.

This is intended for typical queries, particularly those sent frequently.

<br/>

## WebSocket Bulk
A standard WebSocket, except it will accept larger queries and disallows the following:

- `FIND`
- `DELETE`
- `UPDATE`
- `GET`

This interface is intended to store bulk hundreds of objects per query, possibly on startup when initial data is required.

Due to the large query sizes it is intended for high transfer networks rather than over typical internet transfer rates.