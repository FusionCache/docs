---
layout: default
title: Stats
nav_order: 21
parent: API
has_children: false
---

# Stats
Get some basic client and query stats from the server.

This can be sent periodically to monitor client and query stats. The `timestamp` value can be used to calculate average query and client metrics over a time period.

During very high load, this query should be sent less frequently because it will impact query execution performance. Perhaps every few seconds rather than multiple times per second. 


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
The stats include a `totalQueries` value which includes the `STATS` query itself - i.e. if the server starts and the only query received is `STATS`, the `totalQueries` value will be 1 because it includes the `STATS` just received.

<br/>

## Response
`STATS_RSP` object with keys:

| Key           | Type      | Description      |
|:-----         |:---       |:-------          |
| timestamp     | integer  | A Unix timestamp (seconds) of when the stats were created |
| totalQueries  | unsigned int  | Total queries from all interfaces received since startup |
| rest          | object  | Contains `connected` and `total`:<br/>`connected`: unsigned integer - number of clients connected to the REST interface<br/>`total`: unsigned integer - total number of clients that have connected to the REST interface since startup |
| ws          | object  | As `rest` but for the WebSocket interfaces |

<br/>

Query:
```json
{
  "STATS":
  {
  }
}
```

```json
{
  "STATS_RSP":
  {
    "timestamp": 1686054242,
    "totalQueries": 2456321,
    "rest":
    {
      "connected": 0,
      "total": 25
    },
    "ws":
    {
      "connected": 10,
      "total": 10
    }
  }
}
```

This means:

- 25 clients have connected to REST and all have since disconnected
- There have been 10 clients connected to the WebSocket interfaces and all are still connected

If `total > connected` it means a client connected then disconnected before `STATS` was received.

