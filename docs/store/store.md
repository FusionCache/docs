---
layout: default
title: Store
nav_order: 5
parent: API
has_children: true
---

# Store
Store one or more objects.


## Type
Object



## Attributes

| Attribute | Required | Description |
|:-----|:---|:-------|
| _class    | Yes | Either:<br/>Name of a class which must exist<br/>A class definition to create the class |
| _objects  | Yes | `<StoreObjectArray>`<br/>Each object must be the same type as "_class" |
| _rspMode  | No  | "none" or "error" <br/> **Note** "error" is only available for WebSockets |
| _metrics  | No  | Return query metrics |


## Detail
- If `_class` is a definition, the class is created
- The `_objects` array cannot be empty


## Response
`STORE_RSP` containing `<CachedObjectArray>`.

- Each object's key is the `<ClassName>` (the value of `_class` from the `STORE`)
- Each object's value contains an `_oid`, which is the OID for this object

The `_oid` can be used to access an object, for example with `GET`, `UPDATE` and `DELETE`.

<br/>

See [examples](store-examples.md).
