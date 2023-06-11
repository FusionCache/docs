---
layout: default
title: Create Classes
nav_order: 14
parent: API
has_children: false
---

# Create Classes
This query creates one or more classes.

- A class contains at least one member
- Each member must have a data type
- There are two ways to define a member, compact and expanded (see detail)


<br/>


## Type
Object

<br/>

## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _rsp      | No        | Set if a response is required |
| _metrics  | No        | Return query metrics |


<br/>


## Details
`CREATE_CLASSES` contains a JSON object for each class to be created. The key of each object is the class name.

The classes are created in the order they appear in the query, which is useful when a class has a member which is also class:

```json
{
  "CREATE_CLASSES":
  {
    "Access":
    {
      "create":"bool",
      "delete":"bool"
    },
    "Session":
    {
      "id":"string",
      "expire":"int",
      "access":"Access"
    }
  }
}
```


This creates two classes, `Access` then `Session`. Notice the `Session` class has an `access` member whic his of typer `Access`, so the `Access` definition is before `Session` to ensure it exists when the `Session` definition refers to it.

<br/>

If a member requires an expanded definition, for example to set it as indexed, then it must also include `_type`. For example, if we want the `Session::id` to be indexed, we must also set the `_type`:


```json
{
  "CREATE_CLASSES":
  {
    "Access":
    {
      "create":"bool",
      "delete":"bool"
    },
    "Session":
    {
      "id":
      {
        "_type":"string",
        "_index":true
      },
      "expire":"int",
      "access":"Access"
    }
  }
}
```

<br/>



**Compact**

A compact member definition is just the member type:

```json
{
  "CREATE_CLASS":
  {
    "Person":
    {
      "forename":"string",
      "surname":"string"
    }
  }
}
```

`forename` and `surname` are both compact definitions because they only define the data type.


**Expanded**

An expanded definition lets you define the data type and if the member is indexed.

The `_type` must be defined.


```json
{
  "CREATE_CLASS":
  {
    "Person":
    {
      "forename":"string",
      "surname":
      {
        "_type":"string",
        "_index":true
      }
    }
  }
}
```

This defines `forename` and `surname` as a `string`, but now `surname` is indexed.


## Response
`CREATE_CLASSES_RSP` object.

The response contains an object per created class, where the key is the name of the class. 

The response to the above query is:

```json
{
  "CREATE_CLASSES_RSP":
  {
    "Access": {},
    "Session": {}
  }
}
```

No `_err` means both classes were created successfully.