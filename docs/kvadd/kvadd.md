---
layout: default
title: Add
nav_order: 20
parent: KV API
has_children: false
---

# ADD
Stores one or multiple key-values but only if the key does not already exist.

To overwrite an existing key's value, use [`SET`](../kvset/kvset.md).

`ADD` returns a response for each key. If you know that the keys does not already exist, you can use [`ADDQ`](../kvaddq/kvaddq.md) which only returns a response if it fails to add a key.

<br/>

## Structure

`ADD` object with one or many key-value pairs:

```json
{
  "ADD":
  {
    "username":"bob",
    "timeout":60
  }
}
```

This adds keys "username" and "timeout" with their respective values.

<br/>


## Response
`ADD_RSP` object containing the key `k` and status `st`.

<br/>

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeySet
- KeyExists
- KeyLengthInvalid
- KeyTypeInvalid


<br/>

{: .important}
> You will receive a response for each key in `ADD`.
>
> The order of the responses is not gauranteed to be the same as in the `ADD` query.


<br/>

Example:

```json
{
  "ADD_RSP":
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
  "ADD":
  {
    "12345_username":"pingu",
    "12345_email":"pingu@mcemail.com"
  }
}
```

This will return two responses.

