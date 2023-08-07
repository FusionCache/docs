---
layout: default
title: Set
nav_order: 1
parent: KV API
has_children: false
---

# SET
Store one or many key-value pairs. It can also be used to update an existing key's value.

If you want to set a key but don't want to overwrite the value if the key already exists, use [`ADD`](../kvadd/kvadd.md).

<br/>

**Keys**
- A key must be a string
- A key must be at least 6 characters

<br/>

**Values**
- Type can be: string, number or boolean

<br/>


## Structure

An object with key-value pairs: `"<keyname>":<value>`. 

```json
{
  "SET":
  {
    "key1":"astring",
    "key2":234,
    "key3":true
  }
}
```

<br/>


## Response
`SET_RSP` object containing the key  (`k`) and the a status (`st`).

<br/>

| Status  | Meaning | Information      | 
|:---     |:---:    |:---     |
|1        | KeySet            | Key did not already exist, value stored |
|2        | KeyUpdated        | Key existed and its value is updated |
|6        | KeyLengthInvalid  | Error: key is not at least the minimum length |
|7        | TypeInvalid       | Error: key is not a string |


<br/>

{: .important}
> There is a response for each key, so you will receive a response for each key in `SET`.
>
> The order of the responses is not gauranteed to be the same as in the `SET` query.


<br/>

Example:

```json
{
  "SET_RSP":
  {
    "st":1,
    "k":"user1234_username"
  }
}
```

<br/>

## Examples

### Single Pair
```json
{
  "SET":
  {
    "12345_username":"spongebob"
  }
}
```

### Multiple Pairs

```json
{
  "SET":
  {
    "54321_username":"crusty",
    "54321_email":"crusty@mcemail.com",
    "54321_dobyear":1982,
    "54321_active":true
  }
}
```

This produces four separate responses (one for each key in `SET`):
```json
{
  "SET_RSP":
  {
    "k": "54321_active",
    "st": 1
  }
}
```

```json
{
  "SET_RSP":
  {
    "k": "54321_dobyear",
    "st": 1
  }
}
```

```json
{
  "SET_RSP":
  {
    "k": "54321_email",
    "st": 1
  }
}
```

```json
{
  "SET_RSP":
  {
    "k": "54321_username",
    "st": 1
  }
}
```