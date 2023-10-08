---
layout: default
title: Guide - Organising Data
nav_order: 16
has_children: false
---

# Keys

It is a good idea to name keys so they are logically grouped, i.e.

- "app:settings"
- "user:1234:profile"
- "log:queue"

<br/>


# Values

Keys can have a scalar value, such as a string: `"user:1234:username":"billy"`. 

It is generally better for values to be objects because they encapsulate related data.

Let's say we have thousands of user account data cached. Here is the data for two users:


```json
{
  "KV_SET":
  {
    "user:10:username":"billy",
    "user:10:email":"billy@email.com",
    "user:10:status":"LoggedOut",
    "user:10:accountCreateTimestamp":1696692665,

    "user:11:username":"jimmy",
    "user:11:email":"jimmy@email.com",
    "user:11:status":"LoggedIn",
    "user:11:accountCreateTimestamp":1696692365
  }
}
```

Compare this with using a "User" object:

```json
{
  "KV_SET":
  {
    "user:10":
    {
      "User":
      {
        "username":"billy",
        "email":"billy@email.com",
        "status":"LoggedOut",
        "secondsSinceLastAccess":650,
        "accountCreateTimestamp":1696692665
      }      
    },
    "user:11":
    {
      "User":
      {
        "username":"jimmy",
        "email":"jimmy@email.com",
        "status":"LoggedIn",
        "secondsSinceLastAccess":20,
        "accountCreateTimestamp":1696692365
      }      
    }
  }
}
```

We can retrieve all data on a user with a single key:

```json
{
  "KV_GET":["user:10"]  
}
```

It will also be easier for a client application to deserialise the single `User` result into a User object rather than stitching five separate values.