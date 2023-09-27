---
layout: default
title: Clear
nav_order: 40
parent: KV API
has_children: false
---

# KV_CLEAR
Completely empties the cache.


<br/>

## Structure

An empty object:

```json
{
  "KV_CLEAR":{}
}
```


<br/>

## Response
`KV_CLEAR_RSP` object containing the total number of objects of keys deleted (`cnt`) and the status (`cnt`) which always indicates success (`1`).


<br/>

Example:

```json
{
  "KV_CLEAR_RSP":
  {
    "st": 1,
    "cnt": 3543
  }
}
```

