---
layout: default
title: Query Metrics
nav_order: 2
parent: API
---

# Query Metrics
During development it is useful to record timings during query processing and these metrics are available in the response for most queries.

Metrics allow us to see notable changes in latency during development and users as objects and indexes are stored.

<br/>

To receive metrics, set `"_metrics":true` in the query object - at the same level as the <RootClassName>, **not** inside the root class because not all queries have a root class name:

With this attribute set to true, an `_metrics` object is appended to the response - it is not a child of the `_RSP`, it is sibling a (example below).


<br/>
Queries with a root class:

```json
{
  "FIND":
  {
    "_metrics":true,
    "Customer":
    {
      "surname":"Smith"
    }
  }
}
```

Queries without a root class:

```json
{
  "STORE":
  {
    "_metrics":true,
    "_class":"Session",
    "_objects":
    [
      {
        "userId":"abc123456",
        "timeout":"3423423423"
      }
    ]
  }
}
```



<br/>
A query is processed as: 

1. Query received by the network interface
2. Query pushed on to the query queue
3. Query popped from the queue and sent to the executor
4. Execute the query and create the response
5. Query response passed to the network interface to send

<br/>

All metric durations are **microseconds**.

<br/>

The structure is:

| Metrics     |  Description    | Relevant Queries |
|:-----       |:-------         |:---- |
| _qryQueue   | How long the query was on the query queue | All |
| _executor   | Duration of the executor | All |
| _indexes    | Duration to lookup indexes | `FIND`<br/>`COUNT`<br/>`UPDATE` with terms<br/>`DELETE` with terms |
| _nonIndexes | Duration to search non-indexed terms | Same as _indexes |
| _total      | Total duration | All |


<br/>

`_index` and `_nonIndexes` measure time to search indexes and non-indexes, so these are only relevant for queries that involve searching.


<br/>

## Example

This is an example, it is not intended to reflect real world timings:

- Fusion was running on a laptop (AMD Ryzen 9 5900HX)
- There was only one client which only sent one `FIND`

<br/>

We have a key-value cache:

- A "kv" class has "k" and "v" to store key and value respectively
- The key is a string and is indexed
- The value is a string, not indexed
- Keys are randomly generated email addresses, typically ~ 20 to 35 characters
- There are 999,778 keys (i.e. indexes)
- Values are randomly generated words

<br/>

```json
{
  "FIND":
  {
    "_metrics":true,
    "kv":
    {
      "k": "mckayfloyd@zenthall.com"
    }
  }
}

```

<br/>

The response is:

```json
{
  "FIND_RSP": [
    {
      "kv": {
        "k": "mckayfloyd@zenthall.com",
        "v": "cupidatat dolore",
        "_oid": "5c22fa91-9835-4f7a-9175-d0912da49d6d"
      }
    }
  ],
  "_metrics": {
    "_qryQueue": 9,
    "_executor": 18,
    "_indexes": 7,
    "_nonIndexes": 0,
    "_total": 27
  }
}
```

<br/>
This means:

- The query was on the query queue for 9 microseconds
- The executor took 18 microseconds
- As part of executing, there was an index lookup (since `k` is indexed), which took 7 microseconds
- There were no non-indexed terms in the `FIND` so `_nonIndexes` is 0
- Total time from enqueing the query to when the network interface returns the response was 27 microseconds

<br/>

{: .important}
> Note
>
> `_metrics` is a sibling of `FIND_RSP`, not a child of `FIND_RSP`


