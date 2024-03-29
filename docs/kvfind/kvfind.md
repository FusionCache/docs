---
layout: default
title: Find
nav_order: 31
parent: KV API
has_children: false
---

# KV_FIND
Searches the cache for values matching criteria and returns their keys. 

Note, this is a relatively expensive operation because all keys must be checked. Even when the `keyrgx` is set, each key must still be checked with the regular expression.

{: .important}
> This command is useful but may not be suitable for production. A future release will provide a suitable alternative.

<br/>

### Scalar Values
```json
{
  "KV_SET":
  {
    "user:10:username":"Hector",
    "user:10:age":75,
    "user:11:username":"MrWhite",    
    "user:11:age":55
  }
}
```

Find values that are "Hector":
```json
{
  "KV_FIND":
  {
    "==":"Hector"
  }
}
```

Returns `"user:10:username"`.

<br/>

Find where value is less than 60:

```json
{
  "KV_FIND":
  {
    "<":60
  }
}
```
Returns `"user:11:age"`.


{: .important}
> It is not recommended to use scalar values because they don't provide encaspulation/structure to the data. In the example above, a "User" object with "age" and "username" members is better.

<br/>

### Object Values
You can use `path` to select from inside the object. We create a key `"user:10"` with an object value:

```json
{
  "KV_SET":
  {
    "user:10":
    {
      "User":
      {
        "username":"Hector"
      }
    }
  }
}
```
We can use `path` to select the username from inside the User object:

```json
{
  "KV_FIND":
  {
    "path":"/User/username",
    "==":"Hector"
  }
}
```

<br/>

### Restrict Keys

The examples above checks values for all keys, but we may want to restrict which values are checked. The `keyrgx` is used for this.

The `keyrgx` is a regular expression string which must match the whole key (i.e. it won't match substrings).

Example: we track web and phone app users. We format our keys as `<app_type>:user:<user_id>`:


```json
{
  "KV_SET":
  {
    "web:user:10":
    {
      "User":
      {
        "username":"webuser10",
        "loggedIn":true
      }
    },
    "web:user:11":
    {
      "User":
      {
        "username":"webuser11",
        "loggedIn":false
      }
    },
    "phone:user:100":
    {
      "User":
      {
        "username":"phoneuser100",
        "loggedIn":true
      }
    },
    "phone:user:101":
    {
      "User":
      {
        "username":"phoneuser101",
        "loggedIn":true
      }
    }
  }
}
```

We want to find all web users who are logged in:

```json
{
  "KV_FIND":
  {
    "keyrgx":"web:user:[0-9]+",
    "path":"/User/loggedIn",
    "==":true
  }
}
```
<br/>

This returns just `web:user:10`.


- `keyrgx` filters the keys to "web:user:" followed by a number
- `path` selects `/User/loggedIn`
- `==` checks if `/User/loggedIn` equals `true`

<br/>

## Operator

The operator is mandatory and defines which comparison to perform. It can be one of:

  - `==`
  - `>`
  - `<`
  - `>=`
  - `<=`

<br/>


## Structure 
The path is optional but is required if the values is an object.


| Name | Description | Required |
|:---  |:--- |:---:|
| `keyrgx`    | string: a regular expression. Only keys which match the expression will have their value checked. Cannot be empty. | N |
| `path`      | string: a JSON Pointer path to select from **objects**. Cannot be empty. | N |


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

We can't use `path` here because `"abc.json"` is a string rather than an object.

To find the keys with a scalar value (string, integer, bool, etc) you should omit the path:

```json
{
  "KV_FIND":
  {
    "==":"abc.json"
  }
}
```

<br/>


## Use Path
The `path` is a JSON Pointer:

```json
{
  "KV_FIND":
  {
    "path":"/User/address/city",
    "==":"London"
  }
}
```

This returns the keys for all values which contain:

```json
{
  "User":
  {
    "address":
    { 
      "city":"London"
    }
  }
}
```

<br/>


## Examples - With Path

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
