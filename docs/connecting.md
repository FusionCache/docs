---
layout: default
title: Connecting
nav_order: 2
parent: API
---


# Connecting

The following apply to both REST and WebSocket interfaces:

- Only HTTP is supported, not HTTPS
- Binary payload is not supported, only JSON (i.e. text)

<br/>


## REST
- The request must be a HTTP GET
- The query must be in the body of the request, **not** the request's parameters
- The Content-Type should be "application/json"

Here's an example of a query sent to localhost from the Postman client software:

![example get request](images/api_connect_rest.png)

<br/>

## WebSockets
The Normal and Bulk WebSocket servers both have the following settings:

| Setting           | Value |
|:---               |:---   |
|Handshake timeout  | 30 seconds|
|Idle timeout       | 300 seconds|
|Keep-alive pings   | true|
|Per-message deflate| true|
|Auto fragment      | false|


The query must be textual data in the WebSocket message.

<br/>

### WebSockets: Handle Responses
As explained in the [Design](design.md#query-response-order), Fusion is asynchronous and read queries are executed concurrently. 

This means if a client submits two read queries close together, there is no guarantee which response the client will receive first. 

For example, if the client submits two `FIND` queries a few microseconds apart, the client must handle the response in any order: the second `FIND` may take less time to execute, so Fusion sends its response first, even though from the client's point of view it hasn't received a response from the first `FIND`.

To help this, the [QueryID](qid.md) can be used: the `_qid` is set by the client and is returned in the response.