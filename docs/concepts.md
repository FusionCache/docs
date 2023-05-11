# Concepts

Fusion is designed to be easy to use with just a few concepts:

- ObjectID (object identifiers)
- Classes
- Relationships
- Query interfaces

These concepts are used in other software or programming languages, so it's a case of how they apply to Fusion. 


<br/>

## Object ID (OID)
Every object stored in the cache is assigned a unique identifier, called an ObjectID, or just OID.

<br />

An OID is a standard UUID v4, allowing Fusion to identify an object by its OID. As OIDs  unique, they can be stored in set and map structures for efficient retrieval, with the OID as a key.

When you store an object to the cache, Fusion assigns it an OID and the OID is returned in the response. You can use that OID with queries such as `GET`, `UPDATE` and `DELETE`.

{: .important}
> It is not possible for users to supply the OID because Fusion cannot be sure it is unique.

<br />

## Classes
A Fusion class is similar to a class in an object orientated programming language in that:

- The class must have a unique name
- A class has members
- Each class member has a data type (string, integer, etc)
- A member's data type can also be a class


To store, delete, update, etc objects, you must tell Fusion the class type. This means that most queries require a class name:


```json

{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"Susan",
        "surname":"Boyle"
      }
    ]
  }
}
```

This query says:

- Store an object
- Its type is `Person`
- `_objects` is therefore an array of `Person` objects
- The `Person` object has:
  - `forename` has a string value "Susan"
  - `surname` hass a string value "Boyle"


Fusion sends a response:

```json
{
  "STORE_RSP":
  [
    {
      "Person":
      {
        "_oid":"04546b65-7176-48c4-83ef-49912b753497"
      }
    }
  ]
}
```

If the `STORE` contained two objects, there would be two objects and two OIDs in the response.

You can use the OID to get, delete or update the object later:

```json
{
  "GET":
  {
    "Person":
    {
      "_oids":["04546b65-7176-48c4-83ef-49912b753497"]
    }
  }
}
```

This query is:

- I want an object from the cache
- The object is a type of Person
- Here is the OID

The response is:

```json
{
  "GET_RSP":
  [
    {
      "Person":
      {
        "forename":"Susan",
        "surname":"Boyle",
        "_oid":"04546b65-7176-48c4-83ef-49912b753497"
      }
    }
  ]
}
```

<br/>

Some final points:
- A query response is the original query name with `_RSP` appended 
- `GET` always returns the OID for each object, even though you must already know the OID to request the object. This is because you can `GET` multiple objects, so you use each OID to any particular objects you need
- If you don't need to know the OID after storing, you can use either:
  - `_rsp:"error"`, which will only have a response if there's an erorr (only available on WebSocket)
  - `_rsp:"none"` , which will prevent Fusion sending a response, including on an error
  - This is also useful if  you are storing many objects, avoiding handling large responses










