---
layout: default
title: Add Quiet
nav_order: 4
parent: KV API
has_children: false
---

# ADDQ
Add Quiet is the same as [ADD](../kvadd/kvadd.md) but a response is only sent if an error occurs.
<br/>

## Structure

`ADDQ` object with one or many key-value pairs:

```json
{
  "ADDQ":
  {
    "username":"frankie",
    "timeout":5
  }
}
```

<br/>


## Response
`ADDQ_RSP` object containing the key `k` and status `st`. The status can only be an error condition.

<br/>

| Status  | Meaning | Information      | 
|:---     |:---:    |:---     |
|4        | KeyExists         | Error: key already exists |
|6        | KeyLengthInvalid  | Error: key is not at least the minimum length |
|7        | TypeInvalid       | Error: key is not a string |


<br/>

{: .important}
> There is a response for each key **with an error condition**.
>
> The order of the responses is not gauranteed to be the same as in the `ADDQ` query.


<br/>

Example - the key already exists:

```json
{
  "ADDQ_RSP":
  {
    "st":4,
    "k":"user1234_username"
  }
}
```
