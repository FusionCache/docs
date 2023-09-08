---
layout: default
title: Indexes
parent: Object Mode
nav_order: 2
has_children: false
---

# Indexes
Indexes are used to reduce latency when identifying objects in the cache. 

Fusion has only one type of index and it must be defined when a class is created. It is not yet possible to create an index after the class is created.

<br/>

## Background
An index is used to reduce lookup latency. This is similar to an index in a book - rather than looking sequentially through each page for a topic, you look at the index to find the page number(s) for that topic. 

In Fusion, the topic is an object member's value (`surname` is "Smith") and the page is an OID.

<br/>


## Performance
Indexes increase store, delete and update latency because indexes have to be managed, but it reduces search time.

For example, we a cache with only 500,000 key-value objects. We do a `FIND` for a `key`: 


```json
{
  "FIND":
  {
    "_metrics":true,
    "kv":
    {
      "k":"someemail@blah.com"
    }
  }
}
```

We can compare execution time with `k` being indexed and not indexed.

The [metrics](api-metrics.md) include time to search index and non-indexed terms, so we can compare the `_indexes` and `_nonIndexes` values. 

- When `k` is indexed, only `_indexes` is relevant
- When `k` is not indexed, only `_nonIndexed` is relevant:


`k` is not indexed:

```json
{
  "_metrics":
  {
    "_qryQueue": 1,
    "_executor": 115713,
    "_indexes": 0,
    "_nonIndexes": 115699,
    "_total": 115749
  }
}
```

`k` is indexed:

```json
{
  "_metrics":
  {
    "_qryQueue": 3,
    "_executor": 30,
    "_indexes": 10,
    "_nonIndexes": 0,
    "_total": 78
  }
}
```


This tells us:

- When `k` is indexed, it takes 10 microseconds to perform the index 
- When `k` is not indexed, it takes 115699 microseconds (~116ms) to search all 500k objects

<br/>

These differences are further realised when comparing a query with multiple terms.


For example, a cache for customer orders with a `Customer` class:

- `Customer::address` is an `Address`
- `Customer::orders` is array of `Order`
- Each `Order` has an `items` member, which is an array of `OrderItem`. 

Find customers who live in "Derwood" that have ordered an item called "Apextri":


```json
{
  "FIND":
  {
    "_metrics":true,
    "Customer":
    {
      "address":
      {
        "city":"Derwood"
      },
      "orders":
      {
        "items":
        {
          "name":"Apextri"
        }
      }
    }
  }
}
```

We compare with `Address::city` and `OrderItem::name` indexed.


No indexes:

```json
{
  "_metrics":
  {
    "_qryQueue": 26,
    "_executor": 62188,
    "_indexes": 0,
    "_nonIndexes": 62082,
    "_total": 62269
  }
}
```


With indexes:

```json
{
  "_metrics":
  {
    "_qryQueue": 1,
    "_executor": 329,
    "_indexes": 152,
    "_nonIndexes": 0,
    "_total": 417
  }
}
```

Without indexes, the `_nonIndexes` search took ~62,000 microseconds (62ms), whilst the indexed search takes 152 microseconds (0.152ms).

<br/>


## Count
A `COUNT` query can contain terms, for example to `COUNT` Customer objects with `surname` of "Smith".

This performs a `FIND` and returns the number of objects found. Because of this, this explanation concentrates on `FIND`.


<br/>


## Find
`FIND` prioritises indexed terms because it can quickly identify objects that meet the search criteria, or conversely, identify if no objects meet the criteria. Indexed terms are always checked before non-indexed terms.


### One Indexed Term
A `Customer` class with an indexed `surname` member:

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

- Lookup the `surname` index
- If none, return an empty response
- Else the objects for "Smith" are returned


<br/>


### One Indexed Term and One Non-Indexed
If `surname` is indexed and `forename` is not indexed:

```json
{
  "FIND":
  {
    "Customer":
    {
      "forename":"John",
      "surname":"Smith"
    }
  }
}
```

 The sequence is:

- Lookup the `surname` index for "Smith"
- If none, return empty response
- Else check if any of the "Smith" objects have `forename` of "John"


<br/>


### Two Indexed Terms and One Non-Indexed
We have a `Customer` class with three members:

- `surname` and `registeredYear` are indexed
- `forename` is not indexed


```json
{
  "FIND":
  {
    "Customer":
    {
      "forename":"John",
      "surname":"Smith",
      "registeredYear":1995
    }
  }
}
```

The sequence is:

1. Lookup the `surname` index, if no results then return empty response
2. Lookup the `registeredYear` index, if no results then return empty response
3. We have a OIDs from `surname` and `registeredYear` index lookups, so do an intersection to find which OIDs are present in both
4. If there are no OIDs from the intersection, return empty response
5. From the intersected OIDs, check which have `forename` of "John"
6. If none then return empty response, otherwise return the objects


<br/>


### Query with nested objects
The same applies when the query includes an associated class:

```json
{
  "FIND":
  {
    "Customer":
    {
      "surname":"Smith",
      "address":
      {
        "city":"London"
      }
    }
  }
}
```

Assuming `Customer::surname` and `Address::city` are indexed:

1. Lookup the `Customer::surname` index for "Smith", if no results return empty response
2. Step 1 returns `Customer` OIDs, so for each of those we get their `Address` OID
3. Lookup the `Address::city` index for "London", if no results return empty response
4. Intersect the `Address` OIDs from step 2 with the OIDs from step 3
5. This tells us the customers returned in step 1 that have `Address::city` of London

