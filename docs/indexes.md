---
layout: default
title: Indexes
nav_order: 3
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
Indexes increase store, delete and update because the index has to be managed, but it will dramatically reduce search time.

For example, a cache with only 500,000 key-value objects. We do a `FIND` for a `key`, we can compare the query metrics with `key` indexed and not indexed.


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


The [metrics](api-metrics.md) include time to search index and non-indexed terms, so we can compare the `_indexes` and `_nonIndexes` values. 

When `k` is indexed, only `_indexes` is relevant, and `k` is not indexed, only `_nonIndexed` is relevant:

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

This tells us:

- When `k` is indexed, it takes 10 microseconds to lookup the index
- When `k` is not indexed, it takes 115699 microseconds (~116ms) to search all 500k objects

About a x10 difference with only 500,000 objects. Or another way of thinking - the executor could have executed another 10 indexed queries during the time it was searching all the objects.


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

The `surname` index is checked for "Smith" entries. If no entries are found an empty response is returned, otherwise the objects for "Smith" are returned. 


<br/>


### One Indexed Term and One Non-Indexed
If there is one indexed term and one non-indexed term:

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
- If it does not exist, return empty response
- Else check if any of the "Smith" objects have `forename` of "John"


<br/>


### Two Indexed Terms and One Non-Indexed
We have a `Customer` class with two indexed members, `surname` and `registeredYear`, and `forename` which is not indexed.


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
3. We have a OIDs from `surname` and `registeredYear`, so do an intersection to find which OIDs are present in both
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

Assuming `surname` and `city` are indexed:

1. Lookup the `surname` index, if no results return empty response
2. For each OID, get its `address` OID (i.e find the `Address` object for each "Smith")
3. Intersect each `Address` OID with the `Address::city` index (i.e. which of the "Smith" addresses have a `city` of "London")
4. Now we know the Address OIDs of only those people with `surname` of "Smith" and `city` of "London"
5. These OIDs are Address oids, so the final stage is to "up join" from `Address` to `Customer`, to get the `Customer` OIDs for those `Address` objects

Step 5 is called an "Up Join" because `Address` is a child of `Customer` (`Customer` **_has an_** an `Address`).

