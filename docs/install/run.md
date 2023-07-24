---
layout: default
title: Run
parent: Install
nav_order: 3
has_children: false
---

# Run Fusion
At least one of the Rest or WebSocket Standard query interfaces must be configured for Fusion to start. The WebSocket Bulk interface is optional.

<br/>

## Config File
The settings are read from a JSON config file. The default config file is:

```json
{
  "restQuery":
  {
    "enabled":true,
    "ip":"127.0.0.1",
    "port":1981,
    "maxRead":1024
  },
  "wsQuery":
  {
    "enabled":true,
    "ip":"127.0.0.1",
    "port":1982,
    "maxRead":1024
  },
  "bulkStore":
  {
    "enabled":false,
    "ip":"127.0.0.1",
    "port":1983,
    "maxRead":8388608
  }
}
```

<br/>

The `restQuery`, `wsQuery` and `bulkStore` all the same settings. All are **required**:

| Parameter       | Type  | Description
|:---             |:---:  | :---  |
|`enabled`        | bool    | true to enable the interface, otherwise false | 
|`ip`             | string  | IP address for the interface |
|`port`           | integer | Port of the interface |
|`maxRead`        | integer | Max size, in bytes, of the payload the interface will accept |


<br/>


## Query Interface Read Buffers
Each query interface has a maximum buffer size for the query. If a query is received that exceeds the maximum size, it is rejected. 

When setting a max read size, remember this size of buffer is allocated **per query**, and is not released until the response is sent.

This likely only matters if you are sending many smaller queries or a few large queries to bulk store.

