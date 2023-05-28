---
layout: default
title: Indexes
nav_order: 14
parent: API
has_children: false
---

# Indexes
Get a list of the indexes in the cache. The response includes the class name, member name and the number of keys. 

An index in Fusion is created on a class member, such as `Customer::surname`. Each indexed member has a structure which maps values to OIDs. For example, `Customer::surname` may have an entry for "Smith" to map the `Customer` objects with `surname` of "Smith" to the OIDs.

The response does not include the number of OIDs in each key, only the number of keys for each indexed member.

<br/>

## Type
Object

<br/>


## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _metrics  | No        | Return query metrics |


<br/>


## Detail
This is used to get information on the indexes in the cache:

For each index it shows:

- The class name
- The member that is indexed
- For each member, how many keys are indexed


<br/>


## Response
`INDEXES_RSP` array of object.

Each object represents a class that has at least one member indexed.

Each member is included with a `_keys` integer which is the number of keys in that index.

For example, if we create classes:

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

We have `Session::id` member is indexed. We then `STORE` two objects:

```json
{
  "STORE":
  {
    "_class":"Session",
    "_objects":
    [
      {
        "id":"user3@email.com",
        "expire":123456789,
        "access":
        {
          "create":true,
          "delete":false
        }
      },
      {
        "id":"user4@email.com",
        "expire":123456789,
        "access":
        {
          "create":false,
          "delete":false
        }
      }
    ]
  }
}
```

We see the `id` for each object is different, so this creates two entries in the `Session::id` index:

```json
{
  "INDEXES_RSP":
  [
    {
      "Session":
      {
        "id":
        {
          "_keys": 2
        }
      }
    }
  ]
}
```

- If we now `STORE` an object with an `id` that is already indexed, the `_keys` will not change because there are still only 2 unique keys

Each key is associated with OIDs but this is not returned in the response.
