---
layout: default
title: Pub Sub API
nav_order: 41
has_children: true
---

# Publish Subscribe API
In this mode, Fusion acts as a lightweight publish-subscribe server, providing a distributed messaging service. 

Clients publish to channels whilst clients that are subscribed to those channels will receive messages.

<br/>

# Commands
The main commands are: `PUB`, `SUB` and `UNSUB` which are publish, subscribe and unsubscribe respectively. 

`PUB` and `SUB` have a boolean `create`  flag which if true will create the channel if it does not already exist.

<br/>

# Interface
Pubsub is available via a WebSockets interface and each command is allocated a buffer with min and max values.

Defaults and limits:

| Name            | Limit       |
|:------          |:---         |
| IP              | 127.0.0.1   |
| Port            | 1990        |
| Message Default | 256 bytes   | 
| Message Min     | 64 bytes    | 
| Message Max     | 8192 bytes  | 

The IP, port and buffer size are configured in the `pubsub` section of the configuration file:

```json
"pubsub":
{
  "data":
  {
    "ip":"127.0.0.1",
    "port":1990,
    "maxRead":256
  }
}
```

<br/>


# Responses
Either an `ERROR` or a command specific response. Both contain a status (`st`) and additional info (`m`) fields.

All commands always send a response except for `PUB` which only sends a response on an error.

<br/>

## General Errors
An `ERROR` is returned only if the JSON is invalid or the cause of the error is unknown.

The status field `st` can be either:

| Status  | Reason            |
|:---:    |:---               |
| 3       | JSON parse error  |
| 4       | Uknown command    |

<br/>

## Command Response
A response is the command name appended with `_RSP`, such as `SUB_RSP`. The response contains two members:

| Key     | Value      |
|:---:    |:---         |
| `st`    | unsigned integer: status |
| `m`     | string: additional information if available, otherwise empty string |


<br/>

The status (`st`) value can be:

| Status  | Reason      |
|:---:    |:---         |
| 1       | Ok/success |
| 2       | Uknown command |
| 3       | Command syntax invalid (keys not present or wrong type) |
| 5       | Unknown error |
| 10      | Channel name invalid (i.e. empty) |
| 11      | Channel name already exists |
| 12      | Channel name does not exist |
| 13      | Channel name not present |
| 14      | Channel name array is not an array or an array item is not a string |
| 21      | Type of the `msg` in `PUB` is unsupported |


