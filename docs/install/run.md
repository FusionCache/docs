---
layout: default
title: Run
parent: Install
nav_order: 3
has_children: false
---

# Run Fusion

By default, Fusion will start in KV mode with the query WebSocket interface bound to 127.0.0.1 on port 1987.


<br/>

## Config File
The default config is:

```json
{
  "cacheMode":"kv",
  "kv":
  {
    "ws":
    {
      "ip":"127.0.0.1",
      "port":1987,
      "maxRead":1024
    }
  },
  "objects":
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
}
```

<br/>

The `restQuery`, `wsQuery` and `bulkStore`:

| Parameter       | Type  | Description
|:---             |:---:  | :---  |
|`enabled`        | bool    | true to enable the interface, otherwise false (not applicable to KV mode)| 
|`ip`             | string  | IP address for the interface |
|`port`           | integer | Port of the interface |
|`maxRead`        | integer | Max size, in bytes, of the payload the interface will accept |


<br/>


## Query Interface Read Buffers
Each query interface has a maximum buffer size for the query. If a query is received that exceeds the maximum size, it is rejected. 

