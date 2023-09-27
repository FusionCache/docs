---
layout: default
title: Get
nav_order: 30
parent: KV API
has_children: false
---

# KV_GET
Get one or many values.


<br/>

## Structure

An array of keys:

```json
{
  "KV_GET":["<key1>","<key2>","<keyN>"]
}
```

<br/>

## Response
`KV_GET_RSP` object containing the status (`st`) and the key-value pair if the key exists.

If an error occurs, such as they key does not exist, the status (`st`) is set accordingly with a `k` member set to the key which produced the error:

```json
{
  "KV_GET_RSP":
  {
    "st":22,
    "k":"ThisKeyDoesNotExist"
  }
}
```

<br/>
These are status names, their integer values are listed [here](../kvstatuslist.md):

- Ok
- KeyNotExist
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> You will receive a response for each key in `GET`.
>
> The order of the responses is not gauranteed to be the same as in the `GET` query.

<br/>

Example:

```json
{
  "KV_GET_RSP":
  {
    "st":1,
    "user1234_username":"mary"
  }
}
```


<br/>

## Examples

### Single Key
```json
{
  "KV_GET":["12345_username"]
}
```

### Multiple Keys

```json
{
  "KV_GET":["54321_dobyear","54321_email"]
}
```

This returns a response for each key:

```json
{
  "KV_GET_RSP":
  {
    "54321_dobyear": 1981,
    "st": 1
  }
}
```

```json
{
  "KV_GET_RSP":
  {
    "54321_email": "crusty@mcemail.com",
    "st": 1
  }
}
```