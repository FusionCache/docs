---
layout: default
title: KV API
nav_order: 7
has_children: true
---

# Key Value API
In KV mode, Fusion acts similar to Redis and memcached: data is stored as key-value pairs. 

See [KV Mode](TODO) for more information.



**General**

- Data is stored using `SET` or `ADD`
- `SET` and `ADD` can store multiple pairs
- Data is retrieved with `GET`
- `GET` can retrieve multiple pairs


**Keys**

- Keys have a minimum length (see [KV Mode](TODO))
- Keys must be a string


**Values**

- A value can be:
  - string
  - number (including floating point)
  - boolean


