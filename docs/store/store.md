---
layout: default
title: Store
nav_order: 3
parent: API
has_children: true
---

# Store

## Purpose
Store one or more objects.


## Type
Object



## Attributes

| Attribute | Required | Description |
|:-----|:---|:-------|
| _class    | Yes | Either: <ol> <li>Name of a class which must exist </li> <li>A class definition to create the class</li></ol> |
| _objects  | Yes | `<StoreObjectArray>` Each object must be the same type as "_class" |
| _rspMode  | No  | "none" or "error" <br/> **Note** "error" is only available for WebSockets |
| _metrics  | No  | Return query metrics |


## Detail
- If `_class` is a definition, the class will be created
- The `_objects` array cannot be empty


## Response
`STORE_RSP` containing `<CachedObjectArray>`.

- Each object's key is the `<ClassName>` (the value of `_class` from the `STORE`)
- Each object's value contains an `_oid`, which is the OID for this object

The `_oid` can be used to access an object, for example with `GET`, `UPDATE` and `DELETE`.

<br/>

### Storing two objects, class has simple members 
```json
{
  "STORE_RSP":
  [
    {
      "<RootClassName>":
      {
        "_oid":"<oid>"
      }
    },
    {
      "<RootClassName>":
      {
        "_oid":"<oid>"
      }
    }    
  ]  
}
```


### Storing one object, class has one class-type members 
Here, the root class has a member which is a class type. The OID for that object is also returned.
An example of this would be a `Person` with a member `
```json
{
  "STORE_RSP":
  [
    {
      "<RootClassName>":
      {
        "_oid":"<oid>",
        "<classTypeMemberName>":
        {
          "_oid":"<oid>"
        }
      }
    }    
  ]  
}
```