---
layout: default
title: Query ID (QID)
nav_order: 3
parent: API
---

# Query ID (QID)
The Query ID is used by clients to map a query to its response. 

The `_qid` can be set in the query and is returned in the query response:

- Must be a string
- Maximum length is 32 characters
- It is placed at the query root, i.e. a child of the query (example see below)
- The value has no meaning to Fusion, it only checks it's a string and doesn't exceed the maximum length


A client can set the `_qid` so when a response is received the client knows the response is for that particular query.



## Example

Query:

```json
{
  "FIND":
  {
    "_qid":"active_sessions",
    "Session":
    {
      "active":true
    }
  }
}
```

Response:

```json
{
  "_qid":"active_sessions",
  "FIND_RSP":
  [
    {
      "Session":
      {
        "userId":"abcdef",
        "lastActive":123456789,
        "failedAuthAttempts":1,
        "active":true
      }
    },
    {
      "Session":
      {
        "userId":"fedcba",
        "lastActive":123456976,
        "failedAuthAttempts":0,
        "active":true
      }
    }
  ]
}
```




