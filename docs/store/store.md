---
layout: default
title: Store
nav_order: 1
parent: API
has_children: true
---

# Store

## Purpose
Store one or more objects.

<br/>

## Type
Object

<br/>

## Attributes

| Attribute | Required | Description |
|:-----|:---|:-------|
| _class    | Yes | Name of class which must exist, or a class definition |
| _objects  | Yes | An array of objects. Each object must be the same type as "_class" |
| _rspMode  | No  | "none" or "error" |


<br/>

## Detail
- If `_class` is a definition, the class will be created
- The `_objects` array cannot be empty

<br/>

## Response
`STORE_RSP`

- An array of objects. Each has an object with `_class` as the key and an `_oid` string. This is the OID of the stored object
- Each object includes the class name and OID for objects that were created for members of `_class` that are classes.
