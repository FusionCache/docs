---
layout: default
title: Concepts
nav_order: 1
parent: Overview
---

# Concepts

Fusion is designed to be easy to use with just a few concepts:

- ObjectID (object identifiers)
- FQL (Fusion Query Language)
- Classes
- Relationships
- Query interfaces

These concepts are used in other software or programming languages, so it's a case of learning how they apply in Fusion. 


<br/>

## Object ID (OID)
Every object stored in the cache is assigned a unique identifier, called an ObjectID, or just OID.

An OID is a standard UUID (version 4), allowing Fusion to identify an object by its OID. As OIDs are unique, they can be used in set and map structures for efficient retrieval.

When an object is stored, Fusion assigns it an OID and the OID is returned in the response. You can use the OID with queries such as `GET`, `UPDATE` and `DELETE`.

{: .important}
> It is not possible for users to supply the OID when storing because Fusion cannot be certain it is unique.

<br/>
<br/>


## Fusion Query Language (FQL)
FQL is JSON with a particular structure.

<br/>
<br/>


## Classes
A Fusion class is similar to a class in an object orientated programming language:

- The class must have a unique name
- A class has members
- Each member has a data type (string, integer, etc)
- A member's data type can also be a class


To store, delete, update, etc objects, you must tell Fusion the class type. This means that most queries require a class name:

<table>
<tr>
<td markdown="1">


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

</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- Store an object
- Type is `Person`
- `_objects` is therefore an array of `Person` objects
- This `Person` object has two members:
  - `forename` - a string value "Susan"
  - `surname` - a string value "Boyle"

</td>
</tr>
</table>


Fusion responds:

<table>
<tr>
<td markdown="1">

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

</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- An array of `Person` objects
- Each object includes the OID assigned to the cached object

</td>
</tr>
</table>


You can use the OID to get, delete or update the object later:


<table>
<tr>
<td markdown="1">

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
</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- Get an object from the cache
- The object is a type of `Person`
- Here is the OID

</td>
</tr>
</table>


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
- `GET` always returns the OID for each object, even though you have known the OID to request the object. This is because you can `GET` multiple objects, and can use the OID in teh response when you need a particular object
- If you don't need to know the OID after storing, you can use either:
  - `"_rsp":"error"` - only responds if there's an erorr (only available on WebSocket)
  - `"_rsp":"none"` - no response, including on an error
  - This is also useful if  you are storing many objects, avoiding handling large responses


<br/>
<br/>


## Relations
As with programming language classes, a member of a Fusion class can be a class type.

For example, if we cache data for people, would could have a `Person` class with forename, surname, city and area, but it is better encaspulate the address data in a separate `Address` class:


<table>
<tr>
<td markdown="1">

```json
{
  "CREATE_CLASSES":
  {
    "Address":
    {
      "city":"string",
      "area":"string"      
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
</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- Use `CREATE_CLASSES` to create two classes
- `Person::address` is a type of `Address`

</td>
</tr>
</table>


When we store a Person and their Address, Fusion creates two objects: one for Person and the other for Address, assigning each object a separate OID.

What's the point? Fusion can manage the relationship between the Person and Address:


<table>
<tr>
<td markdown="1">

```json
{
  "STORE":
  {
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"Sean",
        "surname":"Connory",
        "address":
        {
          "area":"Love Island",
          "city":"New York"
        }
      }
    ]
  }
}
```

</td>
<td markdown="1" style="vertical-align: top; padding: 0">

- Store a `Person` object
- `Person::address` is an `Address`, so we set the `Address` members

</td>
</tr>
</table>

Because Fusion knows `Person` and `Address` are linked, it records the link between these Person and Address objects (using their OIDs).

When you request a Person object, Fusion can also return the Address object:


<table>
<tr>
<td markdown="1">

```json
{
  "GET":
  {
    "Person":
    {
      "_oids":["9f0d0983-686e-42e1-99f4-02c8a003bab1"]
    }
  }
}
```
</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- Assume that the OID was returned in the `STORE_RSP`

</td>
</tr>
</table>

The response is:


<table>
<tr>
<td markdown="1">

```json
{
  "GET_RSP":
  [
    {
      "Person":
      {
        "forename":"Susan",
        "surname":"Boyle",
        "address":
        {
          "Address":
          {
            "area":"Love Island",
            "city":"New York",            
            "_oid":"5838e71e-2a01-4065-9f3a-110433f75097"
          }
        },
        "_oid":"9f0d0983-686e-42e1-99f4-02c8a003bab1"        
      }
    }
  ]
}
```
</td>    
<td markdown="1" style="vertical-align: top; padding: 0">

- The `Person` object is returned with the `Address`
- The `Address` has an OID because it cached as a separate object

</td>
</tr>
</table>


{: .important}
> The `address` includes its class type (`Address`) because of plans to support inheritance.


<br/>

The `Address` is a separate object so we can `GET` only the `Address`:


<table>
<tr>
<td markdown="1" style="vertical-align: top; padding: 10">

```json
{
  "GET":
  {
    "Address":
    {
      "_oids":["5838e71e-2a01-4065-9f3a-110433f75097"]
    }
  }
}
```
</td>    
<td markdown="1" style="vertical-align: top; padding: 10">

```json
{
  "GET_RSP":
  [
    {
      "Address":
      {
        "area":"Love Island",
        "city":"New York",        
        "_oid":"5838e71e-2a01-4065-9f3a-110433f75097"
      },
    }
  ]
}
```
</td>
</tr>
</table>


Finally, this relationship can be used when using `FIND` to search. Let's say we search for all people who have an address in New Yok:


<table>
<tr>
<td markdown="1" width="300" style="vertical-align: top; padding: 10">

```json
{
  "FIND":
  {
    "Person":
    {
      "address":
      {
        "city":"New York"
      }
    }
  }
}
```
</td>    
<td markdown="1" style="vertical-align: top; padding: 10">

- Search for people who live in the city of New York

</td>
</tr>
</table>


<table>
<tr>
<td markdown="1" width="300" style="vertical-align: top; padding: 10">

```json
{
  "FIND":
  {
    "Person":
    {
      "surname":"Boyle",
      "address":
      {
        "city":"New York"
      }
    }
  }
}
```
</td>    
<td markdown="1" style="vertical-align: top; padding: 10">

- Search for people who live in the city of New York **and** have surname "Boyle"

</td>
</tr>
</table>

