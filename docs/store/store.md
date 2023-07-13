---
layout: default
title: Store
nav_order: 10
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


## Basic Usage

Query:
```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"Henry",
        "surname":"Smith"        
      }
    ]
  }
}
```

Response:

```json
{
  "STORE_RSP":
  [
    {
      "Person":
      {
        "_oid": "5934649f-f0bb-43af-ad9a-16a0d215044e"
      }
    }
  ]
}
```

<br/>

## With Class Member

- Create a separate `Address` class to store address data
- Add an `"address":"Address"` member to `Person`


```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"Henry",
        "surname":"Smith",
        "address":
        {
          "city":"London",
          "postcode":"L1 ABC"
        }
      }
    ]
  }
}
```

Response:
```json
{
  "STORE_RSP":
  [
    {
      "Person":
      {
        "address":
        {
          "Address":
          {
            "_oid": "7bf738ce-5999-48cd-ac95-8300410d3403"
          }
        },
        "_oid": "50846d2a-2ab1-4b1c-bbf6-92ebbbb18b8b"
      }
    }
  ]
}
```

There are two OIDs returned:

- One for the `Person` object
- Another for the `Address` object because `Person::address` is a class type


<br/>

## Class Array Member

- Person has `address` defined as `Address[]` so we can store multiple addresses 
- `Address` has a new member `current` which is a bool flag to indicate if current address

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"Henry",
        "surname":"Smith",
        "address":
        [
          {
            "city":"London",
            "postcode":"L1 ABC",
            "current":true
          },
          {
            "city":"Paris",
            "postcode":"FR3 NCH",
            "current": false
          }
        ]
      }
    ]
  }
}
```

<br/>

Response:
```json
{
  "STORE_RSP":
  [
    {
      "Person":
      {
        "address":
        [
          {
            "Address":
            {
              "_oid": "f1e19eb4-99d2-42e3-9314-e916e9fc6449"
            }
          },
          {
            "Address":
            {
              "_oid": "96d6110a-50f3-44e5-98ef-190ca74b73d3"
            }
          }
        ],
        "_oid": "74a2bd7e-e1f4-43b1-94ad-dc11d1b31508"
      }
    }
  ]
}
```

We have two `Address` OIDs because we stored two addresses for the `Person`.

<br/>

See [examples](store-examples.md).
