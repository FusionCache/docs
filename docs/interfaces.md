---
layout: default
title: Object Interfaces
nav_order: 0
has_children: false
parent: Objects API
---


# Object Interfaces
Queries are submitted to REST or WebSocket interfaces. There is one REST interface and two WebSocket interfaces.

Each interface can have its max read buffer size set, with each having a maximum defined by Fusion.

<br/>

## Limits

| Interface   | Default     | Minimum   | Maximum     | Port  |
|:---         |:---         |:---       |:---         |:---:  |
|REST         | 1024 bytes  | 64 bytes  | 8 MB        | 1981  |
|WS Standard  | 1024 bytes  | 64 bytes  | 2 MB        | 1982  |
|WS Bulk      | 8 MB        | 8 MB      | 1 GB        | 1983  |


<br/>

A query is rejected if it exceeds the buffer size.

<br/>

## REST
This is a standard HTTP request/response interface. All queries are accepted on this interface, although it involves establishing a new HTTP connection for each query, it should be avoided if querying frequently.

<br/>


## WebSocket Standard
A standard HTTP connection, upgraded to a WebSocket.

This is intended for typical queries, particularly those sent frequently.

<br/>

## WebSocket Bulk
A standard WebSocket, except it will accept larger queries and disallows the following:

- `FIND`
- `DELETE`
- `UPDATE`
- `GET`

This interface is intended to store hundreds of objects per query, possibly on startup when initial data is required. It is more efficient to send one large or a few medium sized `STORE` queries rather than many small queries.
