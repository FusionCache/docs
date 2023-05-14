---
layout: default
title: API
nav_order: 3
has_children: true
---

# API
The API documentation contains many examples as this is preferred to verbose technical syntax descriptions.

<br/>

## Placeholders
Documenting JSON can become confusing so placeholders are used to represent values:

<br/>


### RootClassName

`<RootClassName>` - the name of the root class.

<br/>

This is the value of `_class`:

```json
{
  "STORE":
  {
    "_class":"Session",
    "_objects":
    [
      {
        "sessionId":"acbdefg",
        "expire":12345678,
        "username":"someusername"
      }
    ]
  }
}
``` 

`Session` is the root class 

<br/>

In queries that don't use `_class`, the root class is the first object that's not a keyword:

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith"
    }
  }
}
```

```Customer``` is the root class.



---

### Oid

```<Oid>```

An OID (a version 4 UUID) as a string.

---

### ClassName
```<ClassName>```

The name of a class, such as:  `"_class":"Person"`, would be represented as: `"_class":"<ClassName>"`

---

### Class Definition
```<ClassDefinition>```

The definition of a class which includes the name and member names and type:

```json
"Person":
{
  "forename":"string",
  "surname":"string",
  "address":"Address"
}
```

---

### Store Object Array
`<StoreObjectArray>`

An array of cached objects:

```json
"_objects":
[
  {
    "forename":"James",
    "surname":"Smith",
    "address":
    {
      "city":"London"
    }
  }
]
```

Is represented as:

```json
"_objects":<StoreObjectArray>
```

---

### Cached Object Array
`<CachedObjectArray>`

An array of objects returned from the cache:

```json
"FIND_RSP":
[
  {
    "Person":
    {
      "forename":"James",
      "surname":"Smith",
      "address":
      {
        "city":"London"
      }
    }
  },
  {
    "Person":
    {
      "forename":"Susan",
      "surname":"Boyle",
      "address":
      {
        "city":"Edinburgh"
      }
    }
  }
]
```

<br/>

Is represented as:

```json
"FIND_RSP":<CachedObjectArray>

```


<br/>

## Notes
All queries are available on the REST and WebSocket interfaces, though WebSockets are generally preffered due to lower latency once the connection is established.

- Query names must be all capitals, i.e `find` and `Find` are invalid, it must be `FIND`

- The query must be in the body for both REST and WebSockets

- There are two WebSocket interfaces: normal and bulk. Each query has an allocated buffer, Bulk should only be used when storing many objects because each query allocates at least 2MB (minimum for bulk) per query, whilst normal has a 2MB maximum and is defaulted to 4KB.

- Some queries such as `STORE` and `UPDATE`, offer `"_rspMode":"error"` to signal a client wants response only if there's an error. This is only available on WebSockets.