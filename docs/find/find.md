---
layout: default
title: Find
nav_order: 3
parent: API
has_children: true
---

# Find

## Purpose
Search the cache.


## Type
Object



## Attributes

| Attribute | Required | Description      |
|:-----     | :---|:-------               |
|"_metrics" | No  | Return query metrics  |


## Detail
See ### for `_metrics` information.

- A `<RootClassName>` must be supplied. This sets the starting point of the search and the objects that will be returned.
- The root class can contain zero or more terms are required.
- If no terms are defined, all objects for the root class are returned
- The `FIND` is recursive. If the root class has members that are an object, those objects are returned, and if those objects also have object members, they're also returned.

<br/>

### Find Terms
A find term is a conditional to identify only objects meeting that criteria.


```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith"
    }
  }
}
```

This says, "Find `Customer` objects that have the `surname` of 'Smith'".

<br/>

**Multiple Terms**

Multiple terms are always a logical `AND` operation:

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "gender":"M"
    }
  }
}
```

This says, "Find `Customer` objects that have the `surname` of 'Smith' `AND` a `gender` of 'M'".

<br/>

**Object Members**

You can access object members by using the root class member name then the nested member name.

With a `Customer` class containing an `address` member that is an `Address`:

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "address":
      {
        "city":"London"
      }
    }
  }
}
```

This says, "Find `Customer` objects with `surname` of 'Smith' `AND` the `Address` object has `city` of 'London'".

<br/>

You can search deeper nested levels:

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "address":
      {
        "city":"London"
      },
      "orders":
      {
        "items":
        {
          "name":"Laptop"
        }
      }
    }
  }
}
```

This says, "Find `Customer` objects with `surname` of 'Smith' `AND` the `Address` object has `city` of 'London'" `AND` has an `Order` with an `Item` with `item` of 'Laptop'.

In plain language, "Find customers with surname Smith who live in London and have ordered a laptop".


<br/>

## Response
`FIND_RSP` containing `<CachedObjectArray>`.

- Each object's key is a `<ClassName>`, the same as the `<RootClassName>`
- Each object's value contains an `_oid`, which is the OID for the object

The `_oid` can be used to access an object, for example with `GET`, `UPDATE` and `DELETE`.

<br/>

The general response structure is:

```json
{
  "FIND_RSP":<CachedObjectArray> 
}
```

