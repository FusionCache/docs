---
layout: default
title: Examples
nav_order: 1
parent: Store
grand_parent: API
---

# Examples


### Store one object, create class with simple member types
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
    {
      {
        "forename":"James",
        "surname":"Smith",
        "age":25,
        "height":165.5,
        "employed":true
      }
    }
  }
}
```


<br/>

### Store one object, class already exists, with simple member types
The `Person` class has string, integer, bool and decimal members:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    {
      {
        "forename":"James",
        "surname":"Smith",
        "age":25,
        "height":165.5,
        "employed":true
      }
    }
  }
}
```


<br/>

### Store two objects, class already exists, with simple member types
The `Person` class has string, integer, bool and decimal members:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    {
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
    }
  }
}
```
