---
layout: default
title: API
nav_order: 5
has_children: true
---

# API
The API documentation contains many examples as this is preferred to verbose syntax descriptions.

<br/>

## Notes

- Query names must be all capitals, i.e `find` and `Find` are invalid, it must be `FIND`
- The query must be in the HTTP body
- Each interface allocates a buffer **per query**. The buffer must be large enough to hold the query or it is rejected. The defaults are listed in [Interfaces](interfaces.md)
- On the WebSocket Standard and Bulk interfaces, write queries, such as `STORE` and `UPDATE`, allow the `"_rsp":"none"` attribute which means they only send a response if an error occurs 

