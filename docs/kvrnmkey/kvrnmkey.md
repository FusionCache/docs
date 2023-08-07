---
layout: default
title: Rename Key
nav_order: 7
parent: KV API
has_children: false
---

# RNM_KEY
Changes a key to a new name.


<br/>


## Structure

An array of objects. Each object represents a key change in form: `"<ExistingKey>":"<NewKey>`.

```json
{
  "RNM_KEY":
  [
    {"user_123email":"user_123_email"}
  ]
}
```

This changes the existing key "user_123email" to "user_123_email".


<br/>

## Response
`RNM_KEY_RSP` object with the status (`st`) and `"<OriginalKey>":"<NewKey">`:


```json
{
  "RNM_KEY_RSP":
  {
    "user_123email": "user_123_email",
    "st": 1
  }
}
```
<br/>

The status `st` can be:

| Status  | Meaning | Information | 
|:---     |:---:    |:---         |
|1        | KeySet                | Key changed |
|3        | KeyNotExist           | Error: Existing key does not exist |
|4        | KeyExists             | Error: new key name already exists |
|6        | KeyLengthInvalid      | Error: key is not at least the minimum length |
|7        | TypeInvalid           | Error: key is not a string |

