---
layout: default
title: Set Quiet
nav_order: 15
parent: KV API
has_children: false
---

# SETQ
Set Quiet is the same as [KV_SET](../kvset/kvset.md) but a response is only sent if an error occurs.


<br/>


## Structure

A `KV_SETQ` object with the same syntax as [KV_SET](../kvset/kvset.md).

<br/>


## Response
`KV_SETQ_RSP` object containing the key  (`k`) and the status (`st`):

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> There is a response for each key **with an error condition**.
>
> The order of the responses is not gauranteed to be the same as in the `KV_SETQ` query.


<br/>


Example - key length is below mininum:

```json
{
  "KV_SETQ_RSP":
  {
    "st":25,
    "k":"short"
  }
}
```
