---
layout: default
title: Key Value vs Objects
nav_order: 2
has_children: false
---



# KV vs Objects
For query throughput, KV offers less latency because keys are less complex, the queries require less parsing and there are no classes or relationships to manage.

However data is not encapsulated. For example, to represent a user with key values we can:

```json
{
  "SET":
  {
    "user1_username":"poweruser",
    "user1_email":"powerguy@mcemail.com",
    "user1_active":true
  }
}
```

To retrieve all of this data we have to request all three keys in either in three separate calls or one call:

```json
{
  "GET":
  [
    "user1_username",
    "user1_email",
    "user1_active"
  ]
}
```

In Objects mode, we can represent the same in a `User` class:

```json
{
  "CREATE_CLASSES":
  {
    "User":
    {
      "username":"string",
      "email":"string",
      "active":"bool"
    }
  }
}
```

To store:

```json
{
  "STORE":
  {
    "_class":"User",
    "_objects":
    [
      {
        "username":"poweruser",
        "email":"powerguy@mcemail.com",
        "active":true
      }
    ]
  }
}
```

This will return a `STORE_RSP` which contains the object's unique ObjectID (OID) which we can use to retrieve the User object:

```json
{
  "GET":
  {
    "User":
    {
      "_oids":["046b908e-9c99-4725-aba7-67c536cf5807"]
    }
  }
}
```

Which returns:

```json
{
  "GET_RSP":
  {
    "_class":"User",
    "_objects":
    [
      {
        "username":"poweruser",
        "email":"powerguy@mcemail.com",
        "active":true,
        "_oid":"046b908e-9c99-4725-aba7-67c536cf5807"
      }
    ]
  }
}
```

We have all the `User` fields with just one call.

<br/>

## Relationships
There are no relationships in KV mode because each key/value pair are completely independant. 

In Object mode, relationships between objects are tracked. For example, if we need to store permissions a user has we can use a `Permissions` class:

```json
{
  "CREATE_CLASSES":
  {
    "Permissions":
    {
      "create":"bool",
      "delete":"bool",
      "update":"bool"
    },
    "User":
    {
      "username":"string",
      "email":"string",
      "active":"bool",
      "permissions":"Permissions"
    }
  }
}
```

We can store a user with their permissions:


```json
{
  "STORE":
  {
    "_class":"User",
    "_objects":
    [
      {
        "username":"poweruser",
        "email":"powerguy@mcemail.com",
        "active":true,
        "permissions":
        {
          "create":true,
          "delete":false,
          "update":true
        }
      }
    ]
  }
}
```

This returns two ObjectID (OIDs), one for the `User` object and one for the `Permissions` object. 

When we `GET` the `User`, the `Permissions` object is included:

```json
{
  "GET":
  {
    "User":
    {
      "_oids":["8dbb4761-b56e-45a2-9c3f-148a44add314"]
    }
  }
}
```

Response:

```json
{
  "GET_RSP":
  {
    "_class":"User",
    "_objects":
    [
      {
        "username":"poweruser",
        "email":"powerguy@mcemail.com",
        "active":true,
        "permissions":
        {
          "Permissions":
          {
            "create":true,
            "delete":false,
            "update":true,
            "_oid":"d07d2ba3-5506-40ca-bf2d-accc82ca3eda"
          }          
        },
        "_oid":"8dbb4761-b56e-45a2-9c3f-148a44add314"
      }
    ]
  }
}
```

The `Permissions` object is returned with the `User` because Fusion knows that the `User` object is linked to that `Permissions` object.