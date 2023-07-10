---
layout: default
title: Oids
nav_order: 15
parent: API
has_children: false
---

# Oids
Requests all OIDs for a class.

{: .important}
> This will result in a large response if there are many objects in the cache.


<br/>


## Type
Object

<br/>

## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------               |
| _class    | Yes       | Class name  |
| _metrics  | No        | Return query metrics  |

<br/>

## Details

General form:

```json
{
  "OIDS":
  {
    "_class":<ClassName>
  }
}
```

<br/>

## Response
`OIDS_RSP` with:

- `_class`: String. The name of the class
- `_oids`: Array of strings. Each string is an OID

Each OID is for an object of type `_class`.

<br/>

General form:

```json
{
  "OIDS_RSP":
  {
    "_class":"<ClassName>",
    "_objects":
    [
      <OidsAsStrings>
    ]
  }
}
```

<br/>

## Example

Query:

```json
{
  "OIDS":
  {
    "_class":"Customer"
  }
}
```

Response:

```json
{
  "OIDS_RSP":
  {
    "_class":"Customer",
    "_oids":
    [
      "be0db3f3-1554-451c-8829-1ec0a275139d",
      "de70fe0e-4e0d-47cb-9b66-929af599d1ff"
    ]
  }
}
```
