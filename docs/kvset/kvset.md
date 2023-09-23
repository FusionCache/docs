---
layout: default
title: Set
nav_order: 10
parent: KV API
has_children: false
---

# SET
Store one or many key-value pairs. It can also be used to update an existing key's value.

If you don't want to overwrite the value if the key already exists, use [`ADD`](../kvadd/kvadd.md).

<br/>

**Keys**
- A key must be a string
- A key must be at least 6 characters

<br/>

**Values**

A list of permitted value types is [here](../keyvalues.md#value-types).

<br/>


## Structure

An object with key-value pairs: `"<keyname>":<value>`. 

```json
{
  "SET":
  {
    "keyforastring":"astring",
    "keyforanint":234,
    "keyforabool":true
  }
}
```

<br/>


## Response
`SET_RSP` object containing the key  (`k`) and the status (`st`):

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeySet
- KeyUpdated
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> You will receive a response for each key in `SET`.
>
> The order of the responses is not gauranteed to be the same as in the `SET` query.


<br/>

Example:

```json
{
  "SET_RSP":
  {
    "st":20,
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

### Multiple Keys

```json
{
  "SET":
  {
    "54321_username":"crusty",
    "54321_email":"crusty@mcemail.com",
    "54321_active":true
  }
}
```

This produces three separate responses (one for each key in `SET`):

```json
{
  "SET_RSP":
  {
    "k": "54321_active",
    "st": 20
  }
}
```

```json
{
  "SET_RSP":
  {
    "k": "54321_email",
    "st": 20
  }
}
```

```json
{
  "SET_RSP":
  {
    "k": "54321_username",
    "st": 20
  }
}
```