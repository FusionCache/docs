---
layout: default
title: API Format
nav_order: 1
parent: API
---

# API Format

Documenting JSON can become confusing because of nested levels and non-standard comments, so placeholders are used to represent values:

The documentation refers to simple and complex types:

- Simple: a type which is not a JSON object or an array of objects. Examples are `string`, `decimal` and an array of `string`
- Complex: a JSON object or an array of objects

<br/>

## Root Class Name

`<RootClassName>` - name of the root class.

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

`Session` is the root class. 

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

## Oid

`<Oid>`

An OID as a string. An OID is a version 4 UUID.

---

## Class Name
`<ClassName>`

The name of a class.

In `STORE`:

`"_class":"Person"`, would be represented as: `"_class":"<ClassName>"`

---

## Class Definition
`<ClassDefinition>`

The definition of a class which includes each member name, type and other attributes:

```json

"Person":
{
  "forename":"string",
  "surname":
  {
    "_type":"string",
    "_index":true
  },
  "address":"Address"
}

```

</br>
For example, in `CREATE_CLASSES`:

```json

{
  "CREATE_CLASSES":
  {
    "Address":
    {
      "city":"string"      
    },
    "Person":
    {
      "forename":"string",
      "surname":"string",
      "address":"Address"
    }
  }
}

```

Is shown as:

```json

{
  "CREATE_CLASSES":
  {
    <ClassDefinition>
    .
    .
    <ClassDefinition>
  }
}

```

---


## Store Object Array
`<StoreObjectArray>`

An array of objects to be stored:

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
"_objects" : <StoreObjectArray>
```

---

## Cached Object Array
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

"FIND_RSP" : <CachedObjectArray>

```
