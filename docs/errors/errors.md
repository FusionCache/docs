---
layout: default
title: Errors
nav_order: 17
parent: API
has_children: false
---

# Errors
Get a list of error codes and descriptions.



<br/>


## Type
Object

<br/>

## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _metrics  | No        | Return query metrics |

<br/>

## Detail
When an error is encountered, Fusion returns an `_err` object with a `_c` and `_a` which are code and additional. The code is an integer, so to help clients map codes to meaning, the `ERRORS` is used.

<br/>

## Response
`ERRORS_RSP` - array of objects.

Each object contains two values:

| Key           | Type      | Description     |
|:-----         |:---       |:-------         |
| c     | unsigned integer  | Error code      |
| d     | string  | Description of the error  |

<br/>

Query:
```json
{
  "ERRORS":
  {
  }
}
```

Example response:

```json
{
  "ERRORS_RSP":
  [
    {
      "c": 0,
      "d": "Class does not exist"
    },
    {
      "c": 1,
      "d": "Class already exists"
    },
    {
      "c": 2,
      "d": "Class definition invalid"
    },
    {
      "c": 3,
      "d": "Type does not exist"
    },
    {
      "c": 4,
      "d": "Type not permitted"
    },
    {
      "c": 5,
      "d": "Type differs from definition"
    },
    {
      "c": 6,
      "d": "Attribute is not allowed"
    },
    {
      "c": 7,
      "d": "Attribute not recognised"
    },
    {
      "c": 8,
      "d": "Query type not valid"
    },
    {
      "c": 9,
      "d": "Query is empty"
    },
    {
      "c": 10,
      "d": "Query not recognised"
    },
    {
      "c": 11,
      "d": "Query not permitted on the interface"
    },
    {
      "c": 12,
      "d": "JSON/FQL key is empty"
    },
    {
      "c": 13,
      "d": "No root class found"
    },
    {
      "c": 14,
      "d": "Class name is invalid syntax"
    },
    {
      "c": 15,
      "d": "_rsp not permitted"
    },
    {
      "c": 16,
      "d": "_rsp value invalid"
    },
    {
      "c": 17,
      "d": "Value exceeds max length"
    },
    {
      "c": 18,
      "d": "Attribute expected"
    },
    {
      "c": 19,
      "d": "Value is empty"
    },
    {
      "c": 20,
      "d": "Array contains unexpected type"
    },
    {
      "c": 21,
      "d": "OID invalid UUID syntax"
    },
    {
      "c": 23,
      "d": "Term expected"
    },
    {
      "c": 24,
      "d": "Expected empty object"
    },
    {
      "c": 25,
      "d": "_confirm value invalid"
    },
    {
      "c": 26,
      "d": "Member name is invalid"
    },
    {
      "c": 27,
      "d": "Member not found"
    },
    {
      "c": 28,
      "d": "Unknown error"
    }
  ]
}
```
