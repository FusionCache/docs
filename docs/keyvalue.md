---
layout: default
title: Key-Value Example
nav_order: 5
---

# Key Value Cache
Fusion can be used to store key-value pairs, similar to other cache software.

The steps are:

1. Create a class to represent the key/value
2. Set the key as an index

<br/>

## Create Class

Create a class "kv" with two members:

- `k`: `string` data type and is indexed
- `v`: `any` data type

Using `any` type means that `v` can be any simple type (i.e. not an array or object). `k` could also be `any` but that's not useful in this example.


```json
{
  "CREATE_CLASSES":
  {
    "kv":
    {
      "k":
      {
        "_type":"string",
        "_index":true
      },
      "v":"any"
    }
  }
}
```

We receive a response:

```json
{
  "CREATE_CLASSES_RSP":
  {
    "kv": {}
  }
}
```

An empty `kv` (i.e. no `_err`) means success. 


## Store Objects
Store three `kv` objects, each with a different value type (`string`, `int` and `decimal`)

```json
{
  "STORE":
  {
    "_class":"kv",
    "_objects":
    [
      {
        "k":"user1_email",
        "v":"user1@email.com"
      },
      {
        "k":"user1_session_expire",
        "v":123456789
      },
      {
        "k":"user1_account_balance",
        "v":300.60
      }
    ]
  }
}
```

This returns:

```json
{
  "STORE_RSP":
  [
    {
      "kv":
      {
        "_oid": "18393a92-0683-40a8-b606-945533e94560"
      }
    },
    {
      "kv":
      {
        "_oid": "bdfde7a5-99d3-4276-b7c5-f5ef83b71d65"
      }
    },
    {
      "kv":
      {
        "_oid": "143c18f2-3d34-4eea-98f6-c682996ff1af"
      }
    }
  ]
}
```

This caches the three objects, each has an ObjectID (`_oid`), which are returned in the response. 


Fusion only returns each object's OID, so we don't know which OID relates to which stored object.

We could send an individual `STORE` for each but for this example we don't need to - our app may store these on initial logon so they are available when required.


### Check Indexes
We can confirm there are three index entries for the `kv` class:

```json
{
  "INDEXES":{}
}
```

Response:
```json
{
  "INDEXES_RSP":
  [
    {
      "kv":
      [
        {
          "k":
          {
            "_keys": 3
          }
        }
      ]
    }
  ]
}
```

Internally, indexed members are arranged by their class, so the response shows the `kv` class with the `k` member and three unique keys. Each of those has a single OID (not shown in the response).


## Find
The user wants their account balance so we need to retrieve the object:

```json
{
  "FIND":
  {
    "kv":
    {
      "k":"user1_account_balance"
    }  
  }
}
```

Response:

```json
{
  "FIND_RSP":
  [
    {
      "kv":
      {
        "k": "user1_account_balance",
        "v": 300.6,
        "_oid": "143c18f2-3d34-4eea-98f6-c682996ff1af"
      }
    }
  ]
}
```

When retreiving cached objects, the `_oid` for each object is always returned. We could store this locally which let's us use `GET` the next time we want the value.


## Get
A `GET` query can be used when you know the OID for the object(s). Now that we have the OID from `FIND_RSP` we can use `GET` to update our `balance` value:

```json
{
  "GET":
  {
    "kv":
    {
      "_oids":["143c18f2-3d34-4eea-98f6-c682996ff1af"]
    }
  }
}
```

This returns:

```json
{
  "GET_RSP":
  [
    {
      "kv":
      {
        "k": "user1_account_balance",
        "v": 200.6,
        "_oid": "143c18f2-3d34-4eea-98f6-c682996ff1af"
      }
    }
  ]
}
```

The balance has changed since we cached the object, perhaps they spent money.

<br/>

## Alternative Solution
The above design has disadvantages:

- Each key-value pair creates a separate object
- To delete all key-values for a user we need to delete all the `kv` objects for that user. In this example that's just three, but realistically it would higher

An alternative is a class which contains all the session values. This moves all the session values into a single object.

Create the class:

```json
{
  "CREATE_CLASSES":
  {
    "Session":
    {
      "email":"string",
      "balance":"decimal",
      "expire":"int"
    }
  }
}
```

We cache the data when the user's session starts:

```json
{
  "STORE":
  {
    "_class":"Session",
    "_objects":
    [
      {
        "email":"user1@email.com",
        "balance":300.60,
        "expire":123456789
      }      
    ]
  }
}
```

This returns:

```json
{
  "STORE_RSP":
  [
    {
      "Session":
      {
        "_oid": "201f3c45-9d64-4517-8084-0603ae94790f"
      }
    }
  ]
}
```

Now we know the OID for this user's session. We can use this with `UPDATE`, `DELETE` and `GET`:

Extend the session:

```json
{
  "UPDATE":
  {
    "Session":
    {
      "_oids":["201f3c45-9d64-4517-8084-0603ae94790f"],
      "expire":987654321
    }
  }
}
```

Get the session to get the latest balance:

```json
{
  "GET":
  {
    "Session":
    {
      "_oids":["201f3c45-9d64-4517-8084-0603ae94790f"]
    }
  }
}
```

Delete the session:

```json
{
  "DELETE":
  {
    "Session":
    {
      "_oids":["201f3c45-9d64-4517-8084-0603ae94790f"]
    }
  }
}
```

<br/>

`GET` has lower latency than `FIND`. In a small cache the difference is negligable but the combination of large indexes, many objects and many requests can make a notable difference.

This is because:

- `GET` is a simpler query to parse than `FIND`
- There is a direct mapping between an OID and its object
- Objects are arranged in the cache by their class

To `GET` the `Session` object, Fusion only has to parse the query then make one lookup in the `Session` OID map.

