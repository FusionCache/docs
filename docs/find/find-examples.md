---
layout: default
title: Examples
nav_order: 1
parent: Find
grand_parent: API
---

# Examples


## Search with one simple term
Find objects with `Customer::surname` == "Smith".

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

<br/>

## Search with multiple simple terms
Find objects with `Customer::surname` == "Smith" `AND` `status` == "Active".

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "status":"Active"
    }
  }
}
```

<br/>

## Search with one simple term and one object term
Find objects with `Customer::surname` == "Smith" `AND` `address::Address::city` == "London".

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

<br/>

## Search with two simple terms and one object term
Find objects with `Customer::surname` == "Smith" `AND` `Customer::status` == "Active" `AND` `address::Address::city` == "London".

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "status":"Active",
      "address":
      {
        "city":"London"
      }
    }
  }
}
```
