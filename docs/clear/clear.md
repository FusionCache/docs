---
layout: default
title: Clear
nav_order: 17
parent: API
has_children: false
---

# Clear
Completely empty the cache, deleting all classes, indexes, relations and objects. 

After this query a call to `COUNT` and `INDEXES` will return no results.

<br/>


## Type
Object

<br/>

## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _confirm     | Yes    | A string which acts as protection against accidental deletion  |
| _metrics  | No        | Return query metrics |


The `_confirm` default value is "IKnowWhatImDoing" and is case sensitive. The puropse is to prevent losing all data from an accidental `CLEAR` query - or at least it is more difficult to unintentionally empty the cache.


<br/>

## Detail
This query completely clears the cache, including classes and indexes. 

Root class is not permitted.

<br/>

## Response
`CLEAR_RSP`

The response is a `CLEAR_RSP` object which contains `_cnt`, an integer value of how many objects were deleted.


```json
{
  "CLEAR_RSP":
  {
    "_cnt": 20465
  }
}
```
