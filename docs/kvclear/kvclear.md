---
layout: default
title: Clear
nav_order: 5
parent: KV API
has_children: false
---

# Clear
Removes **all** keys from the cache.


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
`CLEAR_RSP` object containing the status (`st`) and `cnt` which is the total number of objects of keys deleted.

<br/>

| Status  | Meaning | Information | 
|:---     |:---:    |:---     |
|0        | Ok                | Success |


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




<br/>


## Examples

```json
{
  "CLEAR":
  {
  }
}
```