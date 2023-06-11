---
layout: default
title: Query Errors
nav_order: 5
parent: API
---

# Query Errors
Errors in queries are reported in an specific response.

The response is the query name with `_RSP_ERR` appended (i.e. the normal response with `_ERR` appended):

- A `STORE` error is:  `STORE_RSP_ERR`
- A `CREATE_CLASSES` error is: `CREATE_CLASSES_RSP_ERR`

This is because some responses are arrays whilst others are objects, so it is easier to have a separate response that is always an object.

<br/>

## Error Response

{: .important}
> An error response on the REST interface is sent has the status field set to 400 (Bad Request)

<br/>

An error response is general form:

```json
{
  "<QueryName>_RSP_ERR":
  {
    "_err":
    {
      "_c":<ErrorCode>,
      "_a":<Additional>
    }
  }
}
```

| Key | Description |
|:---|:---|
|`_err`| An object |
|`_c`| Unsigned integer representing the error code |
|`_a`| String, additional information. May be empty|


A list of error codes and descriptions can be requested using the [Errors](errors/errors.md) query.

<br/>

## Example
We send a `FIND` with a typo in the `Address::city` member name:

```json
{
  "FIND":
  { 
    "Person":
    {
      "addresses":
      {
        "citya":"Edinburgh"
      }
    }
  }
}

```

Response:
```json
{
  "FIND_RSP_ERR":
  {
    "_err":
    {
      "_c": 28,
      "_a": "citya"
    }
  }
}
```

