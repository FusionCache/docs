---
layout: default
title: Bulk Store
parent: Guides
nav_order: 2
has_children: false
---

# Bulk Store
Bulk store refers to storing many objects in a single query using the Bulk Store interface. This interface allows larger buffer sizes so you're able to store more objects per query.

This is use case is likely to be when Fusion is started and initial data is required. 

<br/>

## Example
This example uses a file with a single `STORE` query containing 3.75 million "kv" objects.

A snippet from the file:

```json
{
  "STORE":
  {
    "_rsp": "none",
    "_metrics": true,
    "_class":
    {
      "kv":
      {
        "k":
        {
          "_type": "string",
          "_index": true
        },
        "v":
        {
          "_type": "string",
          "_index": false
        }
      }
    },
    "_objects":
    [
      {
        "k": "adelemiller@furnitech.com",
        "v": "nisi"
      },
      {
        "k": "lorrievang@earthmark.com",
        "v": "nulla"
      },
      {
        "k": "frywitt@krog.com",
        "v": "sunt"
      },
      {
        "k": "dellalynch@comverges.com",
        "v": "reprehenderit"
      },
      {
        "k": "loveclements@geeky.com",
        "v": "occaecat"
      },
      {
        "k": "myersrodgers@sealoud.com",
        "v": "laboris"
      },

```

- We really want `"_rsp":"none"` or the server will respond with 3.75 million OIDs for the new objects
- We want `_metrics` for timing
- `kv` is a simple key-value class with:
  - `k` is the key which is a `string` and is indexed
  - `v` is the value which is a `string`
- The keys and values are auto generated
- Because the keys are auto generated email addresses, there aren't 3.75 million unique address, there are some duplicated, so there are actually 3,746,634 unique keys

The file is ~340MB and the test runs on Azure Intel VMs.

<br/>

## Client
The client is a Standard_E2bs_v5, which has only two threads but this isn't important, the client sequence is:

1. Send a `SERVER_INFO`, wait for the response and print to the terminal
2. Read the JSON file into memory
3. Connect to the WS Bulk Store interface then send the query
4. Receive the `STORE_RSP`
5. Send a `COUNT` to get the number of `kv` objects
6. Print metrics and count to the terminal


<br/>

## Server
Fusion is running on a Standard_E48s_v3 which has 384GB of RAM and 48 threads, although the threads aren't important for a `STORE`.

The server is started with:

`./fusionserver --restQueryIp=10.0.0.6 --wsQueryIp=10.0.0.6 --bulkStoreIp=10.0.0.6 --bulkStoreMaxRead=350000000`

<br/>

## Outcome
The `SERVER_INFO_RSP` is:

```json
{
  "SERVER_INFO_RSP": {
    "serverVersion": "0.1.0",
    "userAgent": "FusionCache",
    "resources": {
      "cpu": {
        "threadsFusionMax": 64,
        "threadsAvailable": 48
      },
      "queryEngine": {
        "threads": 47
      },
      "networkIo": {
        "threads": 5
      }
    },
    "interfaces": {
      "rest": {
        "ip": "10.0.0.6",
        "port": 1981,
        "maxReadSize": 1024
      },
      "standard": {
        "ip": "10.0.0.6",
        "port": 1982,
        "maxReadSize": 1024
      },
      "bulk": {
        "ip": "10.0.0.6",
        "port": 1983,
        "maxReadSize": 350000000
      }
    }
  }
}
```


Count:
```json
{
  "COUNT_RSP":
  {
    "kv":
    {
      "_cnt":3750000
    }
  }
}
```

{: .important}
> At the moment, the `_metrics` does not include time to parse the JSON/FQL which is not insigficant with 360MB of JSON but it was ~8.5 seconds. A possible addition is a new attribute in `_metrics` to record JSON/FQL parsing.

<br/>

Metrics:
```json
{
  "_metrics":
  {
    "_qryQueue":53,    
    "_indexes":0,
    "_nonIndexes":0,
    "_executor":7553171,
    "_total":7553266
  }
}
```

This means:

- The query was in the queue for 53 microseconds
- The executor took ~7.6 seconds to store and index 3.75 million objects (excluding parsing the JSON/FQL)

If we query the server from Postman:

Send `INDEXES`:
```json
{
  "INDEXES_RSP":
  [
    {
      "kv":
      {
        "k":
        {
          "_keys": 3746634
        }
      }
    }
  ]
}
```

- There are not 3.75 million keys because the auto generated data has duplicate keys


<br/>

Do a `FIND` for `"k":"mcleodgolden@tribalog.com"`
```json
{
  "FIND_RSP": [
    {
      "kv": {
        "k": "mcleodgolden@tribalog.com",
        "v": "eiusmod laboris culpa",
        "_oid": "d30c9788-3874-4890-9c5e-0d056a4b1b88"
      }
    }
  ],
  "_metrics": {
    "_qryQueue": 3,
    "_executor": 31,
    "_indexes": 21,
    "_nonIndexes": 0,
    "_total": 70
  }
}
```



