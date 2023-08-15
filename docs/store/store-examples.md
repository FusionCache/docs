---
layout: default
title: Examples
nav_order: 1
parent: Store
grand_parent: Objects API
---

# Examples


## Store one object with class definition and simple members
The `Person` class has string, integer, bool and decimal members:

```json
{
  "STORE":
  {
    "_class":
    {
      "Person":
      {
        "forename":"string",
        "surname":"string",
        "age":"int",
        "height":"decimal",
        "employed":"bool" 
      }
    },
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith",
        "age":25,
        "height":165.5,
        "employed":true
      }
    ]
  }
}
```

<br/>

## Store one object with class name and simple members
The `Person` class has string, integer, bool and decimal members:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith",
        "age":25,
        "height":165.5,
        "employed":true
      }
    ]
  }
}
```


<br/>

## Store two objects with class name and simple members
The `Person` class has string, integer, bool and decimal members:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith",
        "age":25,
        "height":165.5,
        "employed":true
      },
      {
        "forename":"Ben",
        "surname":"Bond",
        "age":45,
        "height":135.0,
        "employed":false
      }
    ]
  }
}
```

<br/>

## Store one object with class name and complex member
The `Order` class has a `decimal` member `total` and an array of `Item`.

```json
{
  "STORE":
  {
    "_class":"Order",
    "_objects":
    [
      {
        "total":900.00,
        "items":
        [
          {
            "Item":
            {
              "name":"Laptop",
              "price":700.00
            }
          },
          {
            "Item":
            {
              "name":"Monitor",
              "price":200.00
            }
          }
        ]
      }
    ]
  }
}
```
