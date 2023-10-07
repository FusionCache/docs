---
layout: default
title: Find
nav_order: 31
parent: KV API
has_children: false
---

# KV_FIND
Searches the cache for values matching criteria and returns their keys.

The command can have an optional `path` and requires an operator: `==`, `>`, `<`, `>=`, `<=`.


{: .important}
> This is an expiremental command and syntax.

<br/>

## Structure 
There are two ways to define search criteria: with or without a `path`.

## Path
The `path` is a JSON Pointer:

```json
{
  "KV_FIND":
  {
    "path":"/address/city",
    "==":"London"
  }
}
```

This says, "return the key for all values which contain `{ "address":{"city":"London"} }`.

<br/>

## No Path
If a key's value is not an object you cannot use `path` because the path requires a root object. Let's say we have a `"defaults:config"`:

```json
{
  "KV_SET":
  {
    "defaults:config":"abc.json"
  }
}
```

We can't use JSON pointer here because `"abc.json"` is a string rather than an object.

To find the keys with a scalar value (string, integer, bool, etc) you omit the path (the alternative was to allow `path` to be an empty string but this seemed more error prone):

```json
{
  "KV_FIND":
  {
    "==":"abc.json"
  }
}
```

<br/>


## Example - With Path

Given:

```json
{
  "KV_SET":
  {
    "users:10":
    {
      "forename":"Sarah",
      "surname":"Sporran",
      "dobYear":1985,
      "address":
      {
        "city":"London"
      }
    },
    "users:11":
    {
      "forename":"James",
      "surname":"Jock",
      "dobYear":1990,
      "address":
      {
        "city":"London"
      }
    }
  }
}
```

We find all users who live in London:

```json
{
  "KV_FIND":
  {
    "path":"/address/city",
    "==":"London"
  }
}
```
This returns Sarah and James.

<br/>

Find all users born in 1985:

```json
{
  "KV_FIND":
  {
    "path":"/dobYear",
    "==":1985
  }
}
```

This returns just Sarah.

<br/>


Find all users born before 1990:

```json
{
  "KV_FIND":
  {
    "path":"/dobYear",
    "<":1990
  }
}
```

This returns just Sarah.

<br/>


Find all users born after and including 1985:

```json
{
  "KV_FIND":
  {
    "path":"/dobYear",
    ">=":1985
  }
}
```

This returns Sarah and James.

<br/>

### Arrays

```json
{
  "KV_SET":
  {
    "user:10":
    {
      "user":
      {
        "groups":["Dev", "Test", "Manager"]
      }      
    },
    "user:11":
    {
      "user":
      {
        "groups":["Test", "Manager"]
      }
    },
    "user:12":
    {
      "user":
      {
        "groups":["Test", "Manager"]
      }
    },
    "user:13":
    {
      "user":
      {
        "groups":["Manager"]
      }
    }
  }
}
```

Find users who are only in Manager:

```json
{
  "KV_FIND":
  {
    "path":"/user/groups",
    "==":["Manager"]
  }
}
```

Find users who are in Test and Manager:

```json
{
  "KV_FIND":
  {
    "path":"/user/groups",
    "==":["Test", "Manager"]
  }
}
```


<br/>


## Examples - No Path
This example shows the syntax, it's not the preferred way to organise keys:

```json
{
  "KV_SET":
  {
    "ui:defaults:showSidePane":false,
    "ui:defaults:darkTheme":true,
    "ui:defaults:restoreLastSession":true
  }
}
```

On startup, find only defaults that are `true`:

```json
{
  "KV_FIND":
  {
    "==":true
  }
}
```
<br/>

This example is unrealistic because it returns any key that is `true` which may not be the intention. This is better:

```json
{
  "KV_SET":
  {
    "ui:defaults":
    {
      "showSidePane":false,
      "darkTheme":true,
      "restoreLastSession":true
    }
  }
}
```

And on startup just `KV_GET` the `"ui:defaults"`.

<br/>


## Response
`KV_FIND_RSP` object containing the status (`st`) and an array of keys (`k`).

The key array is empty if no keys are found.

```json
{
  "KV_FIND_RSP":
  {
    "st":1,
    "k":["user:123", "user:1234"]
  }
}
```

<br/>


## Examples

### Using Path

```json
{
  "KV_SET":
  {
    "logs:app:100":
    {
      "timestamp":1665078681,
      "event":"User Authenticate",
      "user":"user1"
    },
    "logs:app:101":
    {
      "timestamp":1665078685,
      "event":"Report Export",
      "user":"user1"
    },
    "logs:app:102":
    {
      "timestamp":1665078686,
      "event":"Delete Account",
      "user":"user2"
    }
  }
}
```

Find for a user:

```json
{
  "KV_FIND":
  {
    "path":"/user",
    "==":"user2"
  }
}
```

Response:

```json
{
  "KV_FIND_RSP":
  {
    "st": 1,
    "k":["logs:app:102"]
  }
}
```


Find before a timestamp:

```json
{
  "KV_FIND":
  {
    "path":"/timestamp",
    "<=":1665078686
  }
}
```

Response:

```json
{
  "KV_FIND_RSP":
  {
    "st": 1,
    "k":
    [
      "logs:app:100",
      "logs:app:101",
      "logs:app:102"
    ]
  }
}
```
<br/>

## No Path

Not using a path has limited value because it doesn't restrict the search, so it's only useful if it makes sense for your values to be scalar.


Default settings for a UI app:

```json
{
  "KV_SET":
  {
    "property:showSidePanel:":false,
    "property:inboxThreadView":false,
    "property:showProfilePic":true
  }
}
```


Get properties that are true at startup:

```json
{
  "KV_FIND":
  {
    "==":true
  }
}
```
