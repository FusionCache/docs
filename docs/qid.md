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
    "_qid":"some_meaningful_value",
    "Customer":
    {
      "surname":"Jones"
    }
  }
}
```

Response:

```json
{
  "_qid":"some_meaningful_value",
  "FIND_RSP":
  [
    {
      "Customer":
      {
        "forename":"Sam",
        "surname":"Jones"
      }
    }
  ]
}
```




