---
layout: default
title: Add
nav_order: 2
parent: KV API
has_children: false
---

# Add
Stores one or multiple key-values but only if the key does not already exist.

If you want to overwrite an existing key's value, use [`SET`](../kvset/kvset.md).


<br/>

## Structure

`ADD` object with one or many key-value pairs:

```json
{
  "ADD":
  {
    "<key1>":<value1>,
    "<key2>":<value2>,
    .
    .
    "<keyN>":<valueN>
  }
}
```

<br/>


## Response
`ADD_RSP` object containing:
- `k` : the key 
- `st` : status

<br/>

| Status  | Meaning | Information      | 
|:---     |:---:    |:---     |
|1        | KeySet            | Key did not already exist, value stored |
|4        | KeyExists         | Error: key already exists |
|6        | KeyLengthInvalid  | Error: key is not at least the minimum length |
|7        | TypeInvalid       | Error: key is not a string |


<br/>

{: .important}
> There is a response for each key, so you will receive a response for each key in `ADD`.
>
> The order of the responses is not gauranteed to be the same as in the `ADD` query.


<br/>

Example:

```json
{
  "ADD_RSP":
  {
    "st":1,
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
