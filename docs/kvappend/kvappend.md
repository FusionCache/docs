---
layout: default
title: Append
nav_order: 36
parent: KV API
has_children: false
---

# KV_APPEND
Appends to an existing array, object or string.

<br/>

## Structure

The structure depends on the type you are appending:

<br/>

### Array
Add items to an array. They are always appended to the end of the array.

If we have:
```json
{
  "KV_SET":
  {
    "numbers":[1,2,3]
  }
}
```

We can append to the array:
```json
{
  "KV_APPEND":
  {
    "numbers":[4,5,6,"string"]
  }
}
```

If we `KV_GET` the "numbers" array we receive:

```json
{
  "KV_GET_RSP":
  {
    "numbers":[1,2,3,4,5,6,"string"],
    "st":1
  }
}
```

<br/>

### Object
Add member(s) to an object. If the member name alreay exists, it is ignored.

Given:
```json
{
  "KV_SET":
  {
    "person_1234":
    {
      "forename":"Lewis"
    }
  }
}
```

We want to add a surname member:

```json
{
  "KV_APPEND":
  {
    "person_1234":
    {
      "surname":"Hamilton",
      "age":38
    }
  }
}
```

Send a `KV_GET` for "person_1234" and response is:

```json
{
  "KV_GET_RSP":
  {
    "person_1234":
    {
      "forename": "Lewis",
      "surname": "Hamilton",
      "age": 38
    },
    "st": 1
  }
}
```

<br/>

### String
Concatenate a string. 

Given:
```json
{
  "KV_SET":
  {
    "mycatsname":"Fluffy"
  }
}
```

Append the string:

```json
{
  "KV_APPEND":
  {
    "mycatsname":"McFluffyFace"
  }
}
```

Send a `KV_GET` for "mycatsname" and response is:

```json
{
  "KV_GET_RSP":
  {
    "mycatsname": "FluffyMcFluffyFace",
    "st": 1
  }
}
```


<br/>

## Response
`KV_APPEND_RSP` object containing the status (`st`) and key (`k`).

These are status names, their integer values are listed [here](../kvstatuslist.md):

- Ok
- KeyNotExist
- KeyLengthInvalid
- ValueTypeInvalid

<br/>

{: .important}
> You will receive a response for each key in `KV_APPEND`.
>
> The order of the responses is not gauranteed to be the same as in the `KV_APPEND` query.


<br/>


## Examples

### Append Multiple Keys of Various Types

Given:
```json
{
  "KV_SET":
  {
    "person1_groups":["Reporter", "Developer"],
    "person1_summary":"Python enthusiast.",
    "person1_access":
    {
      "Kitchen":false,
      "BreakoutArea":false
    }
  }
}
```

We need to make changes to the person:

- Add them to the "Test" group
- Update the summary
- Give access to a new area


```json
{
  "KV_APPEND":
  {
    "person1_groups":["Test"],
    "person1_summary":"Warning, active user of Arch Linux ... ;)",
    "person1_access":{"Library":true}
  }
}
```

Confirm with a `KV_GET` for all keys:

```json
{
  "KV_GET":["person1_groups", "person1_summary", "person1_access"]
}
```

As always with FusionCache, we will receive a response for *each* key:

```json
{
  "KV_GET_RSP": {
    "person1_groups": [
      "Reporter",
      "Developer",
      "Test"
    ],
    "st": 1
  }
}
```

```json
{
  "KV_GET_RSP": {
    "person1_summary": "Python enthusiast.Warning, active user of Arch Linux ... ;)",
    "st": 1
  }
}
```

```json
{
  "KV_GET_RSP": {
    "person1_access": {
      "Kitchen": false,
      "BreakoutArea": false,
      "Library": true
    },
    "st": 1
  }
}
```