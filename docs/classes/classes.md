---
layout: default
title: Classes
nav_order: 13
parent: API
has_children: false
---

# Classes
Get a list of classes in the cache.

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
The `CLASSES` is used to get the names and definitions of all classes in the cache.

<br/>


## Response
`CLASSES_RSP` array of objects.

The response is an array of object, with each object representing a class, which contains an expanded definition for each member.

{: .important}
> Don't rely on the order of the classes in the response.

<br/>

```json
{
  "CLASSES_RSP":
  [
    {
      "Access":
      {
        "create":
        {
          "_type": "bool",
          "_index": false
        },
        "delete":
        {
          "_type": "bool",
          "_index": false
        }
      }
    },
    {
      "Session":
      {
        "access":
        {
          "_type": "Access",
          "_index": false
        },
        "expire":
        {
          "_type": "uint",
          "_index": false
        },
        "id":
        {
          "_type": "string",
          "_index": true
        }
      }
    }
  ]
}
```