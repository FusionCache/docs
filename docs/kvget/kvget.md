---
layout: default
title: Get
nav_order: 3
parent: KV API
has_children: false
---

# GET
Get one or many key-value pairs.


<br/>

## Structure

An array of keys:

```json
{
  "GET":
  [
    "<key1>",
    "<key2>",
    "<keyN>"
  ]
}
```

<br/>

## Response
`GET_RSP` object containing the status (`st`) and a key-value:  `"<keyname>":<value>`.

<br/>

| Status  | Meaning | Information      | 
|:---     |:---:    |:---      |
|0        | Ok | Success |
|3        | KeyNotExist | Error: key does not exist |
|6        | KeyLengthInvalid | Error: key is not at least the minimum length |
|7        | TypeInvalid | Error: key is not a string |


<br/>

{: .important}
> There is a response for each key, so you will receive a response for each key in `GET`.
>
> The order of the responses is not gauranteed to be the same as in the `GET` query.

<br/>

Example:

```json
{
  "GET_RSP":
  {
    "st":0,
    "user1234_username":"mary"
  }
}
```


<br/>

## Examples

### Single Key
```json
{
  "GET":
  [
    "12345_username"
  ]
}
```

### Multiple Pairs

```json
{
  "GET":
  [
    "54321_dobyear",
    "54321_email"
  ]
}
```

This returns a response for each key:

```json
{
  "GET_RSP":
  {
    "54321_dobyear": 1981,
    "st": 0
  }
}
```

```json
{
  "GET_RSP":
  {
    "54321_email": "crusty@mcemail.com",
    "st": 0
  }
}
```