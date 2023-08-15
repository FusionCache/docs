---
layout: default
title: KV API
nav_order: 9
has_children: true
---

# Key Value API
In KV mode, Fusion acts similar to Redis and memcached: data is stored as key-value pairs. 

See [KV Mode](design.md) for more information.



**General**

- Data is stored using `SET` or `ADD`
- `SET` and `ADD` can store multiple keys
- Data is retrieved with `GET`
- `GET` can retrieve multiple keys


**Keys**

- Keys have a minimum length of 6 characters
- Keys must be a string


**Values**

- A value can be:
  - String
  - Number, including floating point
  - Boolean
  - Object
  - Array
    - Items can be string, number, boolean, object or array



For example:

```json
{
  "SET":
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

We can retrieve the key:

```json
{
  "GET":
  [
    "user_654321"
  ]
}
```