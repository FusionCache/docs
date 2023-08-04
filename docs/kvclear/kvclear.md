---
layout: default
title: Clear
nav_order: 5
parent: KV API
has_children: false
---

# Clear
Completely empties the cache.


<br/>

## Structure

An empty object:

```json
{
  "CLEAR":
  {    
  }
}
```


<br/>

## Response
`CLEAR_RSP` object containing the total number of objects of keys deleted (`cnt`) and the status (`cnt`) which is always 0 (Success).


<br/>

Example:

```json
{
  "CLEAR_RSP":
  {
    "st": 0,
    "cnt": 3543
  }
}
```

