---
layout: default
title: Clear
nav_order: 40
parent: KV API
has_children: false
---

# CLEAR
Completely empties the cache.


<br/>

## Structure

An empty object:

```json
{
  "CLEAR":{}
}
```


<br/>

## Response
`CLEAR_RSP` object containing the total number of objects of keys deleted (`cnt`) and the status (`cnt`) which always indicates success (`1`).


<br/>

Example:

```json
{
  "CLEAR_RSP":
  {
    "st": 1,
    "cnt": 3543
  }
}
```

