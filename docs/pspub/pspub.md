---
layout: default
title: Pub
nav_order: 2
parent: Pub Sub API
has_children: false
---

# Publish
Publish to one or many channels.

To reduce network traffic, the server will only respond with a `PUB_RSP` if an error occurs. 

This command can create channels if they don't exist. You can also create a channel without publishing using `CREATE_CHNL`.  

<br/>

## Structure

A `PUB` object with:

| Key               | Type      | Information |  Required |
|:---               |:------    |:---         |:---:       |
| create  | boolean   | `true`: creates the channel(s) if they don't exist <br/> `false`: for each channel that does not exist, a separate error response is returned | N |
| ch      | array of strings  | Channel names to subscribe to. If a channel does not exist, it will be created if `create` is true | Y |
| msg | various | The message that will be sent to subscribers. See below for data types. | Y |

<br/>

## Message Data Types

The message body is set in `msg`. The value type can be any of:

- Boolean
- Number
- String
- Object
- Array

<br/>

**Simple Types (non-structured)**

```json
{
  "PUB":
  {
    "create":true,
    "ch":["channel1"],
    "msg":"this is my string"
  }
}
```

```json
{
  "PUB":
  {
    "create":true,
    "ch":["channel1"],
    "msg":1228.8
  }
}
```

<br/>

**Structured Types**

If the type is object or array, it can contain nested types:

```json
{
  "PUB":
  {
    "create":true,
    "ch":["channel1"],
    "msg":
    {
      "purchase":
      {
        "timestamp":1694170202,
        "buyerId":74682827,
        "sellerId":92872010,
        "totalPrice":649.99,
        "items":
        [
          {
            "name":"Bagpipes",
            "type":"Extra Loud",
            "price":649.99,
            "qty":1
          }
        ]
      }
    }
  }
}
```

<br/>


## Response

***There is only a response to `PUB` if an error occurs.***


`PUB_RSP` object containing the status (`st`) and additional error message (`m`).

`st` cannot be `1` because `PUB_RSP` is only sent if an erorr occurs.


See [responses](../psapi.md#responses) for a list of `st` values (excluding `1`).

<br/>

## Message Delivery
Messages are delivered to subscribers in a `MSG` object:

| Key     | Type      | Information |
|:---     |:------    |:---         |
| ch      | string    | Channel name  |
| msg     | various   | The message that was published. The JSON type is the same in the `PUB` command that published it  |

<br/>

The type of `msg` is the same the original message published. See [Sub](../pssub/pssub.md) for details.

In the example above, subscribers will receive:

```json
{
  "MSG":
  {
    "ch": "channel1",
    "msg":
    {
      "purchase":
      {
        "buyerId": 74682827,
        "items":
        [
          {
            "name": "Bagpipes",
            "price": 649.99,
            "qty": 1,
            "type": "Extra Loud"
          }
        ],
        "sellerId": 92872010,
        "timestamp": 1694170202,
        "totalPrice": 649.99
      }
    }
  }
}
```

<br/>

## Publishing Examples
<br/>

### Single Channel

Publish to a channel "payments": 
```json
{
  "PUB":
  {
    "ch":["payments"],
    "msg":"some data"
  }
}
```

If the channel exist then no response is sent.

We don't set `create` as `true` so if the channel does not exist we receive:

```json
{
  "PUB_RSP":
  {
    "m": "payments",
    "st": 12
  }
}
```

<br/>

### Multiple Channels

Subscribe to "payments":

```json
{
  "PUB":
  {
    "ch":["payments", "stats"],
    "msg":"some data"
  }
}
```

If "payments" exists then no response for that, but if "stats" did not exist then we receive and error for that channel:

```json
{
  "PUB_RSP":
  {
    "m": "stats",
    "st": 12
  }
}
```
