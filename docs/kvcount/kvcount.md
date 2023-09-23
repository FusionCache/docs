---
layout: default
title: Count
nav_order: 36
parent: KV API
has_children: false
---

# COUNT
Returns the number of keys in the cache.


<br/>


## Structure

An empty `COUNT` object.

<br/>

```json
{
  "COUNT":{}
}
```

<br/>

## Response
`COUNT_RSP` object containing the status  (`st`) and the count (`cnt`).


<br/>


```json
{
  "COUNT_RSP":
  {
    "st":1,
    "cnt":879287
  }
}
```