---
layout: default
title: Errors
nav_order: 22
parent: Objects API
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

Error codes (`_c`) are grouped, starting at 1000 (class errors are 1000, 1100 for type errors, 1200 for attributes, etc), up to 5000 which is Unknown.

<br/>

## Example

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
      "c": 1000,
      "d": "Class does not exist"
    },
    {
      "c": 1001,
      "d": "Class already exists"
    },
    {
      "c": 1002,
      "d": "Class definition invalid"
    },
    {
      "c": 1003,
      "d": "Class name is invalid syntax"
    },
    {
      "c": 1100,
      "d": "Type does not exist"
    },
    {
      "c": 1101,
      "d": "Type not permitted"
    },
    {
      "c": 1102,
      "d": "Type differs from definition"
    },
    {
      "c": 1200,
      "d": "Attribute is not allowed"
    },
    {
      "c": 1201,
      "d": "Attribute not recognised"
    },
    {
      "c": 1202,
      "d": "Attribute expected"
    },
    {
      "c": 1300,
      "d": "Query type not valid"
    },
    {
      "c": 1301,
      "d": "Query is empty"
    },
    {
      "c": 1302,
      "d": "Query not recognised"
    },
    {
      "c": 1303,
      "d": "Query not permitted on the interface"
    },
    {
      "c": 1304,
      "d": "No root class found"
    },
    {
      "c": 1305,
      "d": "Term expected"
    },
    {
      "c": 1400,
      "d": "JSON/FQL key is empty"
    },
    {
      "c": 1500,
      "d": "Expected empty object"
    },
    {
      "c": 1501,
      "d": "Value is empty"
    },
    {
      "c": 1503,
      "d": "Value exceeds max length"
    },
    {
      "c": 1600,
      "d": "_rsp not permitted"
    },
    {
      "c": 1601,
      "d": "_rsp value invalid"
    },
    {
      "c": 1700,
      "d": "OID invalid UUID syntax"
    },
    {
      "c": 1800,
      "d": "Member name is invalid"
    },
    {
      "c": 1801,
      "d": "Member not found"
    },
    {
      "c": 5000,
      "d": "Unknown error"
    }
  ]
}
```
