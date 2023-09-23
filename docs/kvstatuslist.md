---
layout: default
title: Interface
nav_order: 5
has_children: false
parent: KV API
---

# Status List

Each command returns a `st` unsigned integer which can be one of the following:


| Value                 | Name      | Description   |
|:---                   |:---       |:---           |
| 1   | Ok                | Success   |
| 2   | OpCodeInvalid     | The opcode in the WebSocket message is not valid. It must be text. |
| 10  | CommandNotExist   | The command is not known |
| 11  | CommandMultiple   | There may be multiple commands. See [below](#commandmultiple).  |
| 12  | JsonInvalid       | Json fails to parse. See [below](#jsoninvalid) |
| 20  | KeySet            | Key is set. `SET` returns KeySet if it's the first time the key is set. <br /> `ADD` returns KeySet if the key is added (i.e. the key did not already exist) |
| 21  | KeyUpdated        | Key has been set by `SET` and it already existed |
| 22  | KeyNotExist       | Key does not exist, i.e. with `GET` or `RMV` |
| 23  | KeyExists         | Key already exists, i.e. with `ADD` which requires the key does not already exist |
| 24  | KeyRemoved        | Key has been removed. Can only be returned by `RMV` |
| 26  | KeyTypeInvalid    | Key is not a string |
| 41  | ValidTypeInvalid  | A value is not a valid [type](keyvalues.md#value-types) |
| 100 | Unknown           | An unknown error, not one of the above. |



<br/>

## Details

### CommandMultiple
This is when there is more than one command, i.e.:

```json
{
  "GET":["somekey"],
  "GET":["anotherkey"]
}
```

This is invalid. In this case it should be sent in two messages or in a single call as:

```json
{
  "GET":["somekey", "anotherkey"]
}
```

<br/>

### JsonInvalid
If the JSON is invalid a special `ERR` response is sent, see [here](#err-response) for format.

For example, if this is received:

```json
{
  "SET":
  {
    "ThisKeyHasNoValue"
  }
}
```

A `SET_RSP` cannot be returned because the JSON must be parsed to realise that this is a `SET`.

<br/>

### Err Response
This occurs when a command-specific can't be returned, typically when the JSON is invalid or an unknown error occur. 

The response is an `ERR` object with:

| Key | Description   |
|:--- |:---           |
| st  | unsigned int: one of the above status, though typically only JsonInvalid or Unknown |
| m   | string: additional information if available. May be empty. |

Example:

```json
{
  "ERR":
  {
    "st":12,
    "m":""
  }
}
