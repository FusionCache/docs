---
layout: default
title: KV API
nav_order: 40
has_children: true
---

# Key Value API
Fusion acts similar to Redis and memcached, storing as key-value pairs, but Fusion uses WebSockets for querying.


**General**

- Data is stored using `KV_SET` or `KV_ADD`
- `KV_SET` and `KV_ADD` can store multiple keys
- Data is retrieved with `KV_GET`
- `KV_GET` can retrieve multiple keys


**Keys**

- A key have a minimum length of 6 characters
- Keys are always strings


**Values**

- A value can be:
  - String
  - Number, including floating point
  - Boolean
  - Object
  - Array
    - Items can be string, number, boolean, object or array

For example:

Store key "username" with a string value "billy":

```json
{
  "KV_SET":
  {
    "username":"billy"
  }
}
```

Store key "username_age" with an integer value:

```json
{
  "KV_SET":
  {
    "username_age":45
  }
}
```

Store key "user_654321" with an object value:

```json
{
  "KV_SET":
  {
    "user_654321":
    {
      "username":"davethefantastic",
      "active":true,
      "address":
      {
        "city":"London",
        "postcode":"SW1 123",
        "features":["helipad", "swimming pool", "dog"],
        "rooms":
        [
          {"living room":{"width":5.0, "length":7.1}},
          {"kitchen":{"width":3.0, "length":4.5}}
        ]
      }
    }    
  }
}
```

Retrieve a single value:

```json
{
  "KV_GET":["user_654321"]
}
```

Retrieve multiple values:

```json
{
  "KV_GET":["user_654321", "username_age"]
}
```


{: .important}
> The second `KV_GET` will send two separate responses, one for each key.

