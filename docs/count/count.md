---
layout: default
title: Count
nav_order: 12
parent: API
has_children: false
---

# Count
Count the number of objects. The query accepts:

- no root class: return the object count for all classes
- root class without terms: return the object for for the root class
- root class with term(s): return the object count for objects that meet the criteria

`COUNT` is implemented using `FIND`, so terms that are indexed are used to lower latency.

<br/>


## Type
Object


## Attributes

| Attribute | Required  | Description      |
|:-----     |:---       |:-------          |
| _metrics  | No        | Return query metrics |

<br/>

## Detail
`COUNT` is used to get the number of objects, with optional root class and criteria. 

<br/>


**No Root Class**
```json
{
  "COUNT":
  {

  }
}
```

This returns the number of objects for each class.

<br/>

**Root Class, No Terms**
```json
{
  "COUNT":
  {
    "OrderItem":
    {
    }
  }
}
```

Returns the number of `OrderItem` objects.

<br/>

**Root Class with Terms**
```json
{
  "COUNT":
  {
    "OrderItem":
    {
      "name":"Coffee"
    }
  }
}
```

Returns the number of `OrderItem` objects with `name` value of "Coffee".

<br/>

## Response
`COUNT_RSP` which is an object.

The contents of the object depends on the original query.

</br>

**No root class**

Each class name is an object key, with a single integer member value `_cnt`:

```json
{
  "COUNT_RSP":
  {
    "<ClassName1>":
    {
      "_cnt":<int>
    },
    "<ClassName2>":
    {
      "_cnt":<int>
    },
    .
    .
    .
    "<ClassNameN>":
    {
      "_cnt":<int>
    }    
  }
}
```

<br/>

**Root Class, No Terms**

The respoonse is as above but with just the root class:

```json
{
  "COUNT_RSP":
  {
    "<RootClassName>":
    {
      "_cnt":<int>
    }    
  }
}
```

<br/>

**Root Class, With Terms**

The response is as above but the `_cnt` only includes objects that meet the criteria.

For example, to find how many customers live in London:

```json
{
  "COUNT":
  {
    "Customer":
    {
      "address":
      {
        "city":"London"
      }
    }
  }
}
```

Response:
```json
{
  "COUNT_RSP":
  {
    "Customer":
    {
      "_cnt":9850
    }    
  }
}
```

Note, the `_cnt` is within the `Customer`, not `Customer::address::Address`:

```json
{
  "COUNT_RSP":
  {
    "Customer":
    {
      "address":
      {
        "Address":
        {
          "_cnt":9850
        }
      }
    }    
  }
}
```

The `_cnt` is always returned at the root class level.