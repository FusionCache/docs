---
layout: default
title: Run
parent: Install
nav_order: 3
has_children: false
---

# Run Fusion

To start:

- `cd /usr/local/bin/fusioncache`
- `./fusionserver --config=default.json`

By default, Fusion will start in KV mode with:

- the query WebSocket interface bound to `127.0.0.1:1987`
- max query size is `64` bytes

The config file has a `mode` which can be:

- `kv` for key value 
- `objects` for object mode
- `pubsub` for publish subscribe


<br/>

## Config File
The default config is shown below. You only need the section for the current mode, but by default all sections are present.

```json
{
  "mode":"kv",
  "kv":
  {
    "ws":
    {
      "ip":"127.0.0.1",
      "port":1987,
      "maxRead":64
    }
  },
  "pubsub":
  {
    "data":
    {
      "ip":"127.0.0.1",
      "port":1990,
      "maxRead":256
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

## Object Mode Settings
Each interface has settings for `ip`, `port` and `maxRead`. Objects mode has an additional `enabled` setting:

<br/>


| Parameter       | Type  | Description
|:---             |:---:  | :---  |
|`ip`             | string  | IP address for the interface |
|`port`           | integer | Port of the interface |
|`maxRead`        | integer | Max size, in bytes, of the payload the interface will accept |
|`enabled`        | bool    | **Object mode only:** true to enable the interface, otherwise false| 


<br/>


## Query Interface Read Buffers
Each query interface has a maximum buffer size for the query. If a query is received that exceeds the maximum size, it is rejected. 

