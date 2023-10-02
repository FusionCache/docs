---
layout: default
title: Count
nav_order: 38
parent: KV API
has_children: false
---

# KV_COUNT
Returns the number of keys in the cache.


<br/>


## Structure

An empty `KV_COUNT` object.

<br/>

```json
{
  "KV_COUNT":{}
}
```

<br/>

## Response
`KV_COUNT_RSP` object containing the status  (`st`) and the count (`cnt`).


<br/>


```json
{
  "KV_COUNT_RSP":
  {
    "st":1,
    "cnt":879287
  }
}
```