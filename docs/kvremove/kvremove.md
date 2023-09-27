---
layout: default
title: Remove
nav_order: 35
parent: KV API
has_children: false
---

# KV_REMOVE
Deletes one or many key-value pairs.


<br/>

## Structure

An array of keys:

```json
{
  "KV_RMV":["<key1>","<key2>","<keyN>"]
}
```

<br/>

## Response
`KV_RMV_RSP` object containing the status (`st`) and key (`k`).

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeyRemoved
- KeyNotExist
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> You will receive a response for each key in `KV_RMV`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_RMV` query.

<br/>

Example:

Key removed:
```json
{
  "KV_RMV_RSP":
  {
    "st":24,
    "k":"user1234_username"
  }
}
```

<br/>


## Examples

```json
{
  "KV_RMV":
  [
    "54321_username",
    "54321_email",
    "54321_dobyear",
    "54321_active"
  ]
}
```

This produces four responses, one for each key.