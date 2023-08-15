---
layout: default
title: Interface
nav_order: 0
has_children: false
parent: KV API
---


# Key Value Interface
Queries are submitted to the WebSocket interface, there is no REST interface available.

The interface has a max read buffer size **per query**, with a maximum defined by Fusion.


## Limits

| Interface   | Default     | Minimum   | Maximum     | Port  |
|:---         |:---         |:---       |:---         |:---   |
|WebSocket    | 64 bytes    | 64 bytes  | 8 MB        | 1987  |

A query is rejected if it exceeds the buffer size.
<br/>


