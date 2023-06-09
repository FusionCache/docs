---
layout: default
title: API
nav_order: 5
has_children: true
---

# API
The API documentation contains many examples as this is preferred to verbose technical syntax descriptions.

<br/>

## Notes
All queries are available on the REST and WebSocket interfaces, though WebSockets are generally preffered due to lower latency once the connection is established.

- Query names must be all capitals, i.e `find` and `Find` are invalid, it must be `FIND`

- The query must be in the body for both REST and WebSockets

- There are two WebSocket interfaces: Standard and Bulk. Each query has an allocated buffer. The defaults are listed in [interfaces](interfaces.md)

- Some queries such as `STORE` and `UPDATE`, offer `"_rspMode":"error"` to signal a client wants response only if there's an error. This is only available on WebSockets.