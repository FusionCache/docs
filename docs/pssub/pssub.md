---
layout: default
title: Sub
nav_order: 1
parent: Pub Sub API
has_children: false
---

# Sub
Subscribe to one or many channels. A response is received for each channel subscribed to.
<br/><br/>
If the response indicates success the client will receive messages published to each subscribed channel.
<br/><br/>
A message is delivered in a `MSG` object with the channel name (`ch`) and message body (`msg`):

```json
{
  "MSG":
  {
    "ch": "chat",
    "msg":
    {
      "authorId": 123456,
      "body": "Hello, World",
      "timestamp": 1694125017
    }
  }
}
```

<br/>

## Structure

A `SUB` object with:

| Key               | Type      | Information |  Required |
|:---               |:------    |:---         |:---:       |
| create  | boolean   | `true`: creates the channel(s) if they don't exist <br/> `false`: for each channel that does not exist, an error is returned | N |
| ch      | array of strings  | Channel names to subscribe to. If a channel does not exist, it will be created if `createIfNotExist` is true | Y |


<br/>

**Example**
```json
{
  "SUB":
  {
    "create":true,
    "ch":["orders"]
  }
}
```

<br/>


## Response
`SUB_RSP` object containing the status (`st`) and additional error message (`m`).

If `st` is `1` (Success) then `m` is always empty.

See [responses](../psapi.md#responses) for a list of `st` values.

<br/>

{: .important}
> There is a response for each channel subscribed, so you will receive a response for each channel in `ch`.
>
> The order of the responses is not gauranteed to be the same as in the query.


<br/>

## Message Delivery
Messages are delivered to subscribers in a `MSG` object:

| Key     | Type      | Information |
|:---     |:------    |:---         |
| ch      | string    | Channel name  |
| msg     | various   | The message that was published. The JSON type is the same in the `PUB` command that published it  |

<br/>

The type of `msg` is the same the original message published. See [Pub](pub.md) for details.

For example, if this is published:

```json
{
  "PUB":
  {
    "ch":["channel1"],
    "msg":"this is my first message"
  }
}
```

It is delivered as:

```json
{
  "MSG":
  {
    "ch": "channel1",
    "msg": "this is my first message"
  }
}
```

`msg` in the `PUB` and `MSG` are both strings.

<br/>

Another example:

```json
{
  "PUB":
  {
    "ch":["profiles"],
    "msg":
    [
      {
        "username":"Whisky",
        "forename":"Sean",
        "surname":"Connery",
        "active":false
      },
      {
        "username":"Bagpipes",
        "forename":"Susan",
        "surname":"Boyle",
        "active":true
      }
    ]
  }
}
```

Is delivered as:

```json
{
  "MSG":
  {
    "ch": "profiles",
    "msg":
    [
      {
        "active": false,
        "forename": "Sean",
        "surname": "Connery",
        "username": "Whisky"
      },
      {
        "active": true,
        "forename": "Susan",
        "surname": "Boyle",
        "username": "Bagpipes"
      }
    ]
  }
}
```

In this case, `msg` is an array of objects.

<br/>

## Subscribing Examples
<br/>

### Single Channel, Not Creating

Subscribe to a channel "payments" and assume it does not exist. 
```json
{
  "SUB":
  {
    "create":false,
    "ch":["payments"]
  }
}
```
Response:
```json
{
  "SUB_RSP":
  {
    "m": "payments",
    "st": 12
  }
}
```

Channel does not exist and `create` is false so this command fails.

<br/>

### Single Channel, Create If Not Exist

Subscribe to "payments" and create if required:

```json
{
  "SUB":
  {
    "create":true,
    "ch":["payments"]
  }
}
```
Response:
```json
{
  "SUB_RSP":
  {
    "m": "",
    "st": 1
  }
}
```

Channel may not have existed but `create` is true so it would have been created. Response is success.
