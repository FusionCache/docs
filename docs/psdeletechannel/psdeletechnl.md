---
layout: default
title: Delete Channel
nav_order: 4
parent: Pub Sub API
has_children: false
---

# Delete Channel
This command deletes a channel.

After a channel is deleted, clients are not disconnected from the server, even if they are only subscribed to the channel that was deleted.

<br/>

## Structure

A `DELETE_CHNL` object with:

| Key   | Type      | Information       |  Required |
|:---               |:------            |:---|:---: |
| name  | string    | The channel name  | Y |

<br/>

```json
{
  "DELETE_CHNL":
  {
    "name":"ch1"
  }
}
```

<br/>


## Response

A `DELETE_CHNL_RSP` response is always sent, containing the status (`st`) and additional information (`m`) if available.

If `st` indicates success, `m` is always empty.

Typical errors in the response are when the channel name already existing or `name` being the wrong type.

See [statuses](../psapi.md#command-response).