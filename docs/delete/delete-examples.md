---
layout: default
title: Examples
nav_order: 1
parent: Delete
grand_parent: API
---

# Examples


## Delete All Classes Objects
This does not define `_oids` so all objects for `Person` are deleted.
If `Person` has members that are a class type (i.e. an Address member), those objects are not deleted because `_deep` is not `true`.
In this example, there are 150 `Person` objects.

Query:

```json
{
  "DELETE":
  {
    "Person":
    {

    }
  }
}
```

Response:

```json
{
  "DELETE_RSP":
  {
    "Person":
    {
      "_cnt":150
    }
  }
}
```

`_cnt` confirms all 150 `Person` objects are deleted.

<br/>

## Delete Root Class Objects and Class Member Objects
Here, we have a `Session` class which has an `accessRights` member, which is an `Access` class, for example:

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

We want to delete all 650 `Session` objects, after which the `Access` objects are not required, so we set `_deep` to `true`:


Query:

```json
{
  "DELETE":
  {
    "Session":
    {
      "_deep":true,
    }
  }
}
```


Response:

```json
{
  "DELETE_RSP":
  {
    "Session":
    {
      "accessRights":
      {
        "Access":
        {
          "_cnt":650
        }
      },
      "_cnt":650,
    }
  }
}
```

Because each `Session` object is linked to an `Access` object, the response reports that 650 `Access` were also deleted.

<br/>


## Delete Objects by OID
Specific objects can be deletes using the `_oids` attribute. The rules for `_deep` still apply, except it on only applies to objects linked to those in `_oids`.


