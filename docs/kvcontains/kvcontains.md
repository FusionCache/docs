---
layout: default
title: Contains
nav_order: 32
parent: KV API
has_children: false
---

# KV_CONTAINS
Checks if one or multiple keys exist.


<br/>

## Structure

An array of keys:

```json
{
  "KV_CONTAINS":["<key1>","<key2>","<keyN>"]
}
```

<br/>

## Response
`KV_CONTAINS_RSP` object containing the status (`st`) and the key (`k`).

<br/>

These are status names, their integer values are listed [here](../kvstatuslist.md):


- KeyNotExist
- KeyExists
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

Find if key "users:1234:email" exists:

```json
{
  "KV_CONTAINS":["users:1234:email"]
}
```
<br/>

```json
{
  "KV_CONTAINS_RSP":
  {
    "st":23,
    "k":"users:1234:email"
  }
}
```

<br/>

{: .important}
> You will receive a response for each key in `KV_CONTAINS`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_CONTAINS` query.
