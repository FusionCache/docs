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

By default, Fusion starts with settings:

- the command WebSocket interface bound to `127.0.0.1:1987`
- max payload size is `1024` bytes


<br/>

## Config File
The default config is:


```json
{
  "version":1,
  "kv":
  {
    "ip":"127.0.0.1",
    "port":1987,
    "maxPayload":1024
  }
}
```

<br/>

| Parameter   | Type  | Description
|:---         |:---:  | :---  |
|version      | unsigned int  | Must be `1` |
|kv           | object        | Settings for key-value WebSocket interface |


<br/>

### KV Settings

| Parameter       | Type  | Description
|:---             |:---:  | :---  |
|`ip`             | string  | IP address of the interface |
|`port`           | unsigned integer | Port of the interface |
|`maxPayload`     | unsigned integer | Max size, in bytes, of the payload the interface will accept. Must be within the min/max [limits](../kvinterfaces.md) |

