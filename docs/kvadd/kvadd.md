---
layout: default
title: Add
nav_order: 20
parent: KV API
has_children: false
---

# KV_ADD
Stores one or multiple key-values but only if the key does not already exist.

`KV_ADD` always returns a response. If you only want a response on failure, use [`KV_ADDQ`](../kvaddq/kvaddq.md).

To overwrite an existing key's value, use [`KV_SET`](../kvset/kvset.md).


<br/>

## Structure

`KV_ADD` object with one or many key-value pairs:

```json
{
  "KV_ADD":
  {
    "username":"bob",
    "timeout":60
  }
}
```

This adds keys "username" and "timeout" with their respective values.

<br/>


## Response
`KV_ADD_RSP` object containing the key `k` and status `st`.

<br/>

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeySet
- KeyExists
- KeyLengthInvalid
- KeyTypeInvalid


<br/>

{: .important}
> You will receive a response for each key in `KV_ADD`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_ADD` query.


<br/>

Example:

```json
{
  "KV_ADD_RSP":
  {
    "st":20,
    "k":"user1234_username"
  }
}
```

<br/>

## Examples

```json
{
  "KV_ADD":
  {
    "12345_username":"pingu",
    "12345_email":"pingu@mcemail.com"
  }
}
```

This will return two responses.

