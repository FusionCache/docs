---
layout: default
title: Run
parent: Install
nav_order: 3
has_children: false
---

# Run Fusion
At least one of the Rest or WebSocket Standard query interfaces must be configured for Fusion to start. The WebSocket Bulk interface is optional.

- An interface is enabled by supplying an IP address
- Only the IP address must be set, the ports have default values

<br/>

## Query Interface IP Addresses

| Parameter       | |
|:---             |:---     |
|`restQueryIp`    | IP address for the REST query interface |
|`wsQueryIp`      | IP address for the WebSocket Standard query interface |
|`bulkStoreIp`    | IP address for the WebSocket Bulk Store interface |

<br/>

## Query Interface Ports

| Parameter         | Default | |
|:---               |:---:    |:---         |
|`restQueryPort`    |1981     | The port address for the REST query interface |
|`wsQueryPort`      |1982     | The port address for the WebSocket Standard query interface |
|`bulkStorePort`    |1983     | The port address for the WebSocket Bulk Store interface |

<br/>

## Query Interface Read Buffers
Each query interface has a maximum buffer size for the query. If a query is received that exceeds the maximum size, it is rejected. 

When setting a max read size, remember this size of buffer is allocated **per query**, and is not released until the response is sent.

This likely only matters if you are sending many smaller queries or a few large queries to bulk store.

| Parameter             | Default (bytes) | Notes |
|:---                   |---:           |:---     |
|`restQueryMaxRead`     |4096           |         |
|`wsQueryMaxRead`       |4096           |         |
|`bulkStoreMaxRead`     |36'700'160     | 35Mb    |

<br/>

## Examples

<br/>
Rest only, with default port and read buffer size:

`./fusionserver --restQueryIp=10.0.0.1`

<br/>
Rest and WS Standard, with custom read buffer sizes:

`./fusionserver --restQueryIp=10.0.0.1 --wsQueryIp=10.0.0.1 --restQueryMaxRead=256 --wsQueryMaxRead=1024`

<br/>
WS query and bulk only with custom buffer sizes (1024 bytes and 100MB):

`./fusionserver --restQueryIp=10.0.0.1 --wsQueryIp=10.0.0.1 --restQueryMaxRead=1024 --wsQueryMaxRead=104857600`

<br/>
Enable all interfaces. Rest and WS Standard have default buffer sizes, bulk has 100MB.

`./fusionserver --restQueryIp=10.0.0.1 --wsQueryIp=10.0.0.1  --bulkStoreIp=10.0.0.1 --bulkStoreMaxRead=104857600`
