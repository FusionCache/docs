---
layout: default
title: Delete
nav_order: 8
parent: API
has_children: true
---

# Delete
Delete object(s) from the cache.

This frees the memory used by the object(s) in the cache.

<br/>

## Type
Object

<br/>

## Attributes

| Attribute | Required | Description      |
|:-----     | :---|:-------               |
| _oids     | No  | Array of OIDs for objects to delete  |
| _deep     | No  | Boolean<br/>Default: `false`<br/> `false`: delete root class objects only <br/> `true`: delete recursively  
| _metrics  | No  | Return query metrics  |


<br/>

## Detail
- A `<RootClass>` must be supplied
- The `_oids` array must be for objects of `<RootClass>` type
- No terms are permittted


If `_deep` is true then all associated objects are recursively deleted:

- If `_oids` is set then each root class object is deleted and all the objects associated to each OID in `_oids` are deleted
- If `_oids` is not set then all root class object is deleted, and all objects associated to **any** root class object is also deleted

See [examples](delete-examples.md).

<br/>

## Response
A `DELETE_RSP` is returned as:

```json
{
  "DELETE_RSP":
  {
    <RootClass>:
    {
      "_cnt":<int>
    }    
  }
}
```

Where `_cnt` value is the number of objects deleted.


For example:

Query:

```json
{
  "DELETE":
  {
    "Person":
    {
      "_oids":["81291d75-f4ba-45e7-bcbb-01ffe3377066"]
    }
  }
}
```

Response:

```json
{
  "DELETE_RSP":
  {
    "Person":
    {
      "_cnt":1
    }
  }
}
```