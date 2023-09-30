---
layout: default
title: Status Values
nav_order: 5
has_children: false
parent: KV API
---

# Response Status Values

Each command returns a `st` unsigned integer which can be one of the following:


| Value                 | Name      | Description   |
|:---                   |:---       |:---           |
| 1   | Ok                | Success   |
| 2   | OpCodeInvalid     | The opcode in the WebSocket message is not valid. It must be text. |
| 3   | JsonInvalid       | Json fails to parse. See [below](#jsoninvalid) |
| 10  | CommandNotExist   | The command is not known |
| 11  | CommandMultiple   | There may be multiple commands. See [below](#commandmultiple).  |
| 12  | CommandType       | The command is the wrong type, i.e. <br/> `"KV_SET":["abc"]` <br/> where `KV_SET` should be an object: <br/> `"KV_SET":{"somekey":"value"}` |
| 20  | KeySet            | Key is set. `KV_SET` returns KeySet if it's the first time the key is set. <br /> `KV_ADD` returns KeySet if the key is added (i.e. the key did not already exist) |
| 21  | KeyUpdated        | Key has been set by `KV_GET` and it already existed |
| 22  | KeyNotExist       | Key does not exist, i.e. with `KV_GET` or `KV_RMV` |
| 23  | KeyExists         | Key already exists, i.e. with `KV_ADD` which requires the key does not already exist |
| 24  | KeyRemoved        | Key has been removed. Can only be returned by `KV_RMV` |
| 26  | KeyTypeInvalid    | Key is not a string |
| 41  | ValidTypeInvalid  | A value is not a valid [type](keyvalues.md#value-types) |
| 100 | Unknown           | An unknown error, not one of the above. |



<br/>

## Details

### CommandMultiple
This is when there is more than one command, i.e.:

```json
{
  "KV_GET":["somekey"],
  "KV_SET":{"anotherkey":"blah"}
}
```

This is invalid and should be sent in two separate commands.


<br/>

### JsonInvalid
If the JSON is invalid a special `KV_ERR` response is sent, see [here](#err-response) for format.

For example, if this invalid JSON is received:

```json
{
  "KV_SET":
  {
    "ThisKeyHasNoValue"
  }
}
```

A `SET_RSP` cannot be returned because the JSON must be parsed to realise that this is a `SET`.

<br/>

### Err Response
This occurs when a command-specific response can't be returned, typically when the JSON is invalid or an unknown error occurs.

The response is an `KV_ERR` object with:

| Key | Description   |
|:--- |:---           |
| st  | unsigned int: one of the above status, though typically only JsonInvalid or Unknown |
| m   | string: additional information if available. May be empty. |

<br/>
Example:

```json
{
  "KV_ERR":
  {
    "st":3,
    "m":""
  }
}
```