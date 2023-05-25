---
layout: default
title: Get
nav_order: 9
parent: API
has_children: true
---

# Get

## Purpose
Get an object(s) from the cache with the OID.

<br/>


## Type
Array



## Attributes
| Attribute | Required | Description      |
|:-----     | :---|:-------               |
| _oids     | Yes   | Array of OIDs for objects to retrieve  |
| _metrics  | No    | Return query metrics  |


## Details
`GET` will return objects from their OID. The OID is typically by calls to `STORE` and `FIND`.

General form:

```json
{
  "GET":
  {
    "<RootClass>":
    {
      "_oids":<OidArray>
    }
  }
}
```

<br/>

## Response
`GET_RSP`, an array of the objects for the OIDs.

General form:

```json
{
  "GET_RSP":
  [
    <CachedObjects>
  ]
}
```

<br/>

## Example

A `Session` class with `id`, `expire` and `accessRights` members which are a `string`, `int` and `Access` types respectively.

The `Session` objects are returned in an array, with their associated `Access` object. 


Query:

```json
{
  "GET":
  {
    "Session":
    {
      "_oids":
      [
        "be0db3f3-1554-451c-8829-1ec0a275139d",
        "de70fe0e-4e0d-47cb-9b66-929af599d1ff"
      ]
    }
  }
}
```

Response:

```json
{
  "GET_RSP":
  [
    {
      "id": "user3@email.com",
      "expire": 123456789,
      "accessRights":
      {
        "Access":
        {
          "create": true,
          "delete": false,
          "_oid": "3ac5ceef-b1b1-4caa-a3b9-00d5e339d2c3"
        }
      },
      "_oid": "be0db3f3-1554-451c-8829-1ec0a275139d"
    },
    {
      "id": "user4@email.com",
      "expire": 123456789,
      "accessRights":
      {
        "Access":
        {
          "create": false,
          "delete": false,
          "_oid": "ea9a18eb-cd04-4284-a17c-c13bc78d81c7"
        }
      },
      "_oid": "de70fe0e-4e0d-47cb-9b66-929af599d1ff"
    }
  ]
}
```
