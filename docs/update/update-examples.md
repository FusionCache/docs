---
layout: default
title: Examples
nav_order: 1
parent: Update
grand_parent: Objects API
---

# Examples


## Update two objects by OID
Set `expire` to a new value for two objects:

```json
{
  "UPDATE":
  {
    "Session":
    {
      "_oids":
      [ 
        "81291d75-f4ba-45e7-bcbb-01ffe3377066",
        "11241d75-e4ba-41e7-acbb-11bbe3375061"
      ],
      "expire":987654321
    }
  }
}
```

<br/>

## Update all objects
Set `blocked` to `true` for all `Session` objects: 


```json
{
  "UPDATE":
  {
    "Session":
    {
      "blocked":true
    }
  }
}
```

<br/>

## Update root object and class members
Set all Customer's with an `Address` to the city of "London".

```json
{
  "UPDATE":
  {
    "Customer":
    {
      "adddress":
      {
        "city":"London"
      }
    }
  }
}
```

As opposed to this, which updates all Address objects, even if they're not linked to a Customer:

```json
{
  "UPDATE":
  {
    "Address":
    {
      "city":"London"
    }
  }
}
```

This is a subtle difference, but may be important if there are `Address` objects that are not linked to a `Customer` that should not to be updated.