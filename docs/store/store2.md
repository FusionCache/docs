---
layout: default
title: Store2
nav_order: 2
parent: API
---

# Store

## Purpose
Store one or more objects.

## Type
Object

<br/>

## Attributes
|Attribute|Required|Description|
|-----|---|-------|
_class|Yes|Name of class which must exist, or a class definition|
_objects|Yes|An array of objects. Each object must be the same type as "_class"
_rspMode|No|"none" or "error"|


<br/>


## Response
`STORE_RSP`

- An array of objects. Each has an object with `_class` as the key and an `_oid` string. This is the OID of the stored object
- Each object includes the class name and OID for objects that were created for members of `_class` that are classes.

<br/>

## Detail
- If `_class` is a definition, the class will be created
- The `_objects` array cannot be empty


<br/>

## Examples

<br/>

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

### Store two object, class already exists, with simple member types
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
