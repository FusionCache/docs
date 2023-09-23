---
layout: default
title: Set Quiet
nav_order: 15
parent: KV API
has_children: false
---

# SETQ
Set Quiet is the same as [SET](../kvset/kvset.md) but a response is only sent if an error occurs.


<br/>


## Structure

A `SETQ` object with the same syntax as [SET](../kvset/kvset.md).

<br/>


## Response
`SETQ_RSP` object containing the key  (`k`) and a status (`st`) which can only contain an error condition.

<br/>

| Status  | Meaning | Information      | 
|:---     |:---:    |:---     |
|6        | KeyLengthInvalid  | Error: key is not at least the minimum length |
|7        | TypeInvalid       | Error: key is not a string |


<br/>

{: .important}
> There is a response for each key **with an error condition**.
>
> The order of the responses is not gauranteed to be the same as in the `SETQ` query.


<br/>


Example - key length is below mininum:
```json
{
  "SETQ_RSP":
  {
    "st":6,
    "k":"short"
  }
}
```
