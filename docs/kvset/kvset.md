---
layout: default
title: Set
nav_order: 10
parent: KV API
has_children: false
---

# KV_SET
Store one or many key-value pairs. It can also be used to update an existing key's value.

`KV_SET` always returns a response. If you only want a response on failure, use [`KV_SETQ`](../kvsetq/kvsetq.md).

If the key may already exists and and you don't want to overwrite the value, use [`KV_ADD`](../kvadd/kvadd.md).

<br/>

**Keys**
- A key must be a string
- A key must be at least 6 characters

<br/>

**Values**

A list of permitted value types is [here](../keyvalues.md#value-types).

<br/>


## Structure

A `"KV_SET"` object which contains keys and their values. 

You can store a single key/value:

```json
{
  "KV_SET":
  {
    "keyforstring":"astring",
  }
}
```

Stores key `keyforstring` with value `"astring"`.

<br/>

You can also store multiple keys in a single query:

```json
{
  "KV_SET":
  {
    "keyforint":234,
    "keyfordecimal":25.7654321,
    "keyforbool":true,
    "keyforarray":[10, 5.5, {"x":true}],
    "keyforobject":
    {
      "User":
      {
        "username":"BillyBob",
        "email":"bb@email.com",
        "address":
        {
          "city":"Farmville"
        }
      }
    }
  }
}
```

This stores four keys: `"keyforint"`, `"keyfordecimal"`, `"keyforbool"`, `"keyforarray"` and `"keyforobject"`.

<br/>


## Response
`KV_SET_RSP` object containing the key  (`k`) and the status (`st`):

These are status names, their integer values are listed [here](../kvstatuslist.md):

- KeySet
- KeyUpdated
- KeyLengthInvalid
- KeyTypeInvalid

<br/>

{: .important}
> You will receive a response for each key in `KV_SET`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_SET` query.


<br/>

Example:

```json
{
  "KV_SET_RSP":
  {
    "st":20,
    "k":"user1234_username"
  }
}
```

<br/>

## Examples

### Single Key
```json
{
  "KV_SET":
  {
    "user:12345:username":"spongebob"
  }
}
```

This returns a response:

```json
{
  "KV_SET_RSP":
  {
    "st":20,
    "k":"user:12345:username"
  }
}
```

The status (`st`) confirms the set was successful for the given key (`k`).

<br/>

### Multiple Keys

```json
{
  "KV_SET":
  {
    "user:54321:username":"crusty",
    "user:54321:email":"crusty@mcemail.com",
    "user:54321:active":true
  }
}
```

This produces three separate responses (one for each key in `SET`):

```json
{
  "KV_SET_RSP":
  {
    "k": "user:54321:active",
    "st": 20
  }
}
```

```json
{
  "KV_SET_RSP":
  {
    "k": "user:54321:email",
    "st": 20
  }
}
```

```json
{
  "KV_SET_RSP":
  {
    "k": "user:54321:username",
    "st": 20
  }
}
```