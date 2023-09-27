---
layout: default
title: Add Quiet
nav_order: 25
parent: KV API
has_children: false
---

# KV_ADDQ
Add Quiet is the same as [KV_ADD](../kvadd/kvadd.md) but a response is only sent if an error occurs.
<br/>

If you want to overwrite an existing key's value you can use [KV_SET](../kvset/kvset.md).

<br/>

## Structure

`KV_ADDQ` object with one or many key-value pairs:

```json
{
  "KV_ADDQ":
  {
    "username":"frankie",
    "timeout":5
  }
}
```

<br/>


## Response
`KV_ADDQ_RSP` object containing the key `k` and status `st`. The status can only be an error condition because a response is not sent if the key is successfully added.

<br/>
These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeyExists
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> There is a response for each key **that had an error condition**. Keys added without error do not trigger a response.
>
> The order of the responses is not gauranteed to be the same as in the `KV_ADDQ` query.


<br/>

Example - the key already exists:

```json
{
  "KV_ADDQ_RSP":
  {
    "st":23,
    "k":"user1234_username"
  }
}
```
