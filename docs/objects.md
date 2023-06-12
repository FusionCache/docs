---
layout: default
title: Objects
parent: "Overview"
nav_order: 2
---

# Objects
An object in Fusion is the same as an object in all/most programming languages: it's an instance of a class. A class defines the structure and we create an object of that class to hold values.

Each object has an Object ID, also called an OID, which is a version 4 Universally Unique ID (UUID). 

An OID is created by Fusion when an object is stored and cannot be changed. It also not possible for users to supply an OID because Fusion must ensure they are unique.


## Structure
Rather than storing all objects in a single cache, they are grouped by their class. This means queries require the class name (known as the root class) - it needs to know which cache to access.


When an object is stored:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith"
      }
    ]        
  }
}
```

An OID is generated for that object and a mapping from the OID to the object is created:


![Oid to objects map](images/objects_1_storetobject.png)



If the `Person` class has an `address` member which is an `Address` type:

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith",
        "address":
        {
          "city":"Paris"
        }
      }
    ]        
  }
}
```

Two objects are created, one for `Person` and another for `Address`. Each has a unique OID:

<br/>

![Oid to objects map](images/objects_2_storetobject.png)


<br/>

The response to the `STORE` query is a `STORE_RSP` which contains the OID for the `Person` and `Address` objects. 

Fusion manages the relationship between the `Person` and `Address` OIDs. This means when you retrieve the `Person` object using its OID, the `Address` can also be returned.


Query:
```json
{
  "GET":
  {
    "Person":
    {
      "_oids":["94e57e91-5161-41fd-9c17-82279aa4f7dc"]
    }
  }
}
```

Response:
```json
{
  "GET_RSP":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Smith",
        "address":
        {
          "Address":
          {
            "city":"Paris"
          }          
        }
      }
    ]
  }
}
```
The link between the `Person` and `Address` OID is tracked by Fusion, so the `Address` object is returned with the `Person`.

<br/>

## Relationships
Creating separate objects allows Fusion to track the relationships and for objects to be retrieved, deleted and updated separately. In the example above, the `Address` can be updated so the `city` is changed from "London" to "Paris":

```json
{
  "UPDATE":
  {
    "Address":
    {
      "_oids":["4295a815-b983-4cbb-a97d-1859c785d84f"],
      "city":"Paris"
    }
  }
}
```

Now the James Smith lives in Paris - because the updated `Address` OID is linked to the `Person` object for James Smith.