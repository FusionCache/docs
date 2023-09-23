---
layout: default
title: Interface
nav_order: 0
has_children: false
parent: KV API
---


# Key Value WebSocket Interface
Queries are submitted to a WebSocket interface.

The interface has a max read buffer size **per query**, with an absolute maximum defined by Fusion.

<br/>

- HTTPS is not supported, only HTTP is supported
- Binary payload is not supported, only text

<br/>

## Query Buffer Limits

| Default     | Minimum   | Maximum     | Port  |
|:---         |:---       |:---         |:---   |
| 1024 bytes  | 64 bytes  | 2 MB        | 1987  |

A query is rejected if it exceeds the max payload size.

<br/>

## WebSockets Settings

| Name              | Value |
|:---               |:---   |
|Idle timeout       | 180 seconds |
|Per-message deflate| disabled |
|Keep-alive pings   | true |

