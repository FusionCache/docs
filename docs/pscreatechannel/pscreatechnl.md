---
layout: default
title: Create Channel
nav_order: 3
parent: Pub Sub API
has_children: false
---

# Create Channel
This command creates a channel. This is an alternative to creating a channel during `SUB` or `PUB`. 

<br/>

## Structure

A `CREATE_CHNL` object with:

| Key   | Type      | Information       |  Required |
|:---               |:------            |:---|:---: |
| name  | string    | The channel name  | Y |

<br/>

```json
{
  "CREATE_CHNL":
  {
    "name":"ch1"
  }
}
```

<br/>


## Response

A `CREATE_CHNL_RSP` response is always sent, containing the status (`st`) and additional information (`m`) if available.

If `st` indicates success, `m` is always empty.

Typical errors in the response are when the channel name already existing or `name` being the wrong type.

See [statuses](../psapi.md#command-response).