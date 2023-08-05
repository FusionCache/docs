---
layout: default
title: Remove
nav_order: 4
parent: KV API
has_children: false
---

# Remove
Deletes one or many key-value pairs.


<br/>

## Structure

An array of keys:

```json
{
  "RMV":
  [
    "<key1>",
    "<key2>",
    "<keyN>"
  ]
}
```

<br/>

## Response
`RMV_RSP` object containing the status (`st`) and key (`k`).

<br/>

| Status  | Meaning | Information | 
|:---     |:---:    |:---     |
|5        | KeyRemoved        | Key removed                 |
|3        | KeyNotExist       | Error: key does not exist   |
|6        | KeyLengthInvalid  | Error: key is not at least the minimum length |
|7        | TypeInvalid       | Error: key is not a string  |

<br/>

{: .important}
> There is a response for each key, so you will receive a response for each key in `RMV`.
>
> The order of the responses is not gauranteed to be the same as in the `RMV` query.

<br/>

Example:

```json
{
  "RMV_RSP":
  {
    "st":5,
    "k":"user1234_username"
  }
}
```

<br/>


## Examples

```json
{
  "RMV":
  [
    "54321_username",
    "54321_email",
    "54321_dobyear",
    "54321_active"
  ]
}
```