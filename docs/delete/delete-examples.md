---
layout: default
title: Examples
nav_order: 1
parent: Delete
grand_parent: Objects API
---

# Examples

When objects are deleted, the `_deep` attribute determines if only the root class objects are deleted (default action) or if it is recursive.

Recursive is with `_deep` set `true`, which means all objects associated with each deleted root class object are deleted.

If the root class objects have a class member that doesn't make sense to exist without its root class, then recursive delete is the answer. For example, a `Session` class which uses an `Access` class to cache a user's access rights during the session. When the `Session` is deleted, it is not necessary to retain their access rights, so when deleting a `Session`, the `_deep` is set `true` to also delete the `Access` object.


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

## Delete Deep
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
Specific objects can be deletes using the `_oids` attribute.

The rules for `_deep` still apply, except it on only applies to the objects associated to those in `_oids`.

We leave set `_deep` as its default `false`, so we'll only delete the two `Session` objects.


Query:

```json
{
  "DELETE":
  {
    "Session":
    {
      "_oids":
      [
        "2d2e9b15-85d3-458e-8cf5-01bd3989df2f",
        "0931def2-b2ed-4430-b05d-1c7063235463"
      ]
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
      "_cnt": 2
    }
  }
}
```

After this, the `Access` objects for the deleted `Session` objects still exist, which may not be what we wanted.

We should have set `_deep` as `true`:

```json
{
  "DELETE":
  {
    "Session":
    {
      "_deep":true,
      "_oids":
      [
        "a8effe99-47f8-48a5-9455-0adf32531ccc",
        "ff386509-9c08-46ec-b088-a452e80e6f46"
      ]
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
      "access":
      {
        "Access":
        {
          "_cnt": 2
        }
      },
      "_cnt": 2
    }
  }
}
```

`_cnt` is 2 for both because we specified two `Session` objects, each of which has one `Access` member (`Session::accessRights`).