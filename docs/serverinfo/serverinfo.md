---
layout: default
title: Server Info
nav_order: 21
parent: API
has_children: false
---

# Server Info
Returns information about the Fusion server, including version, threads and network interfaces.

<br/>

## Type
Object

<br/>


## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _metrics  | No        | Return query metrics |


<br/>


## Detail
See Response.

<br/>


## Response
`SERVER_INFO_RSP` object.

The response includes objects with specific keys detailed below:

| Key       | Type    | Description      |
|:-----           |:---     |:-------          |
| serverVersion   | string  | The version of the server |
| userAgent       | string  | This value appears in the "Server" HTTP response header |
| resources       | object  | Information for resources available to Fusion (see below) |
| interfaces      | object  | Information on active query interfaces |


<br/>

### Resources
The `resources` object contains the following:

<br/>

`cpu`

Information about CPU resources:
- `threadsFusionMax` : The maximum threads Fusion supports. This is not a technical limit, it will increas as testing progresses.
- `threadsAvailable` : The number of threads reported by the host (the CPU, VM or Docker container). This will be the same as `threadsFusionMax` unless it exceeds `threadsFusionMax` (i.e. if your CPU has more logical cores than Fusion supports)

<br/>

`queryEngine`

The query engine is what executes queries. A query executes on a thread until its completion (excluding sending the response).
- `threads` : The number of threads available for the query engine's executors. This is typically `threadsAvailable-1`, unless `threadsAvailable` is 1, in which case this value is also 1

Read queries are executed concurrently, so `threads` is the maximum concurrent read queries.

<br/>

`networkIo`

Query interfaces use threads to handle client connections, receiving queries and sending responses. Each interface can run use multiple threads or share threads.

- `threads` : The total number of threads available to the network interfaces. The proportion of I/O to available threads may change as testing progresses

The response does not show the number of threads available per interface because they can share threads.

<br/>

### Interfaces

For each interface that is active, the IP, port and maximum read buffer size is given.

Interfaces are `rest`, `standard` and `bulk`. At least one of `rest` or `standard` must be configured so it is not possible to only have `bulk` in this list.

Within each of these, the following are returned:

- `ip` : The IP address of the interface
- `port` : The port of the interface
- `maxReadSize` : The size in bytes of the interface's read buffer 

If a query is received that exceeds `maxReadSize` for the interface, it is rejected.


Example response. This version of Fusion supports up to 64 threads but the host CPU has 12:

<br/>

## Example

```json
{
  "SERVER_INFO_RSP":
  {
    "serverVersion": "0.1.0",
    "userAgent": "FusionCache",
    "resources":
    {
      "cpu":
      {
        "threadsFusionMax": 64,
        "threadsUsable": 12
      },
      "queryEngine":
      {
        "threads": 11
      },
      "networkIo":
      {
        "threads": 2
      }
    },
    "interfaces":
    {
      "rest":
      {
        "ip": "0.0.0.0",
        "port": 1981,
        "maxReadSize": 4096
      },
      "standard":
      {
        "ip": "0.0.0.0",
        "port": 1982,
        "maxReadSize": 4096
      },
      "bulk":
      {
        "ip": "0.0.0.0",
        "port": 1983,
        "maxReadSize": 36700160
      }
    }
  }
}
```