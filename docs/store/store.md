---
layout: default
title: Store
nav_order: 2
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
| _class    | Yes | Name of class which must exist, or a class definition |
| _objects  | Yes | An array of objects. Each object must be the same type as "_class" |
| _rspMode  | No  | "none" or "error" |



## Detail
- If `_class` is a definition, the class will be created
- The `_objects` array cannot be empty


## Response
`STORE_RSP`

- An array of objects. Each has an object with `_class` as the key and an `_oid` string. This is the OID of the stored object
- Each object includes the class name and OID for objects that were created for members of `_class` that are classes


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