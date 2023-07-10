---
layout: default
title: Update
nav_order: 12
parent: API
has_children: true
---

# Update
Update one or more objects.


## Type
Object



## Attributes

| Attribute | Required | Description |
|:-----|:---|:------- |
| _oids     | No      | Array of OID(s) to update |
| _rspMode  | No      | "none" or "error" <br/> **Note** "error" is only available for WebSockets |
| _metrics  | No      | Return query metrics |


## Detail
- Requires a root class name
- If `_oids` is present then the updates are applied to those objects only
- If `_oids` is not present then updates are applied to all `<ClassName>` objects


{: .important}
> If `UPDATE` does not include `_oids` then all root class objects are updated, which may not be the intention. 


There should be one JSON object with the `<ClassName>` as the key. Its value is a list of member names and values to overwrite the existing value.


```json
{
  "UPDATE":
  {
    "Customer":
    {
      "_oids":["912aed75-f4ba-45e7-bcbb-01ffe3377066"],
      "surname":"NewSurname"
    }
  }
}
```

- The OID is for a `Customer` object
- Change this object's `surname` to "NewSurname"

<br/>

You can also update members that are classes. For example, a `Customer` class with an `address` member that's an `Address` type:

```json
{
  "UPDATE":
  {
    "Customer":
    {
      "_oids":["912aed75-f4ba-45e7-bcbb-01ffe3377066"],
      "address":
      {
        "city":"Paris"
      }
    }
  }
}
```

This updates this Customer object's `address` to have a `city` value of "Paris".


<br/>

## Response
`UPDATE_RSP`

The response includes a `_cnt` unsigned integer which is the number of `_class` objects updated.

- When `_oids` is set, the `_cnt` is the size of the `_oids` array.
- When `_oids` is not set, the `_cnt` is the number of `<ClassName>` objects.


On success:

```json
{
  "UPDATE_RSP":
  {
    "<ClassName>":
    {
      "_cnt":<unsigned int>
    }
  }
}
```

<br/>

See [examples](update-examples.md).
