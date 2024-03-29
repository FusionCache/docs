---
layout: default
title: Key Values
nav_order: 10
has_children: false
---

# Key Values
This is associating (i.e. mapping) a unique key to a value.

For example, we could associate `"user123_email"` with `"james123@email.com"` to store the email address for "user123". The key is "user123_email" can be used to retrieve the key later.


<br/>

# Value Types
A value can be:

- string
- number
- boolean
- object 
- array
- null

Binary is not permitted.

<br/>

```json
"user123_surname":"Smith"
```

```json
"user123_age":35
```

```json
"user123_account_active":true
```


```json
"user123_address":
{
  "city":"Paris",
  "current":false
}
```

```json
"favourite_pets":["dog", "cat", "gorilla"]
```

Because a value can be an object, you can store this user's details in a single key:

```json
"user1234_details":
{
  "forename":"James",
  "surname":"Smith",
  "account_active":true,
  "addresses":
  [
    {
      "city":"Paris",
      "current":false
    },
    {
      "city":"London",
      "current":true
    }    
  ],
  "favourite_pets":["dog", "cat", "gorilla"]
}
```
<br/><br/>


# Store a Value
Key-value pairs are stored using the `KV_SET` command:

```json
{
  "KV_SET":
  {
    "user123_surname":"Smith"
  }
}
```
<br/>

```json
{
  "KV_SET":
  {
    "user1234_details":
    {
      "forename":"James",
      "surname":"Smith",
      "account_active":true,
      "addresses":
      [
        {
          "city":"Paris",
          "current":false
        },
        {
          "city":"London",
          "current":true
        }    
      ],
      "favourite_pets":["dog", "cat", "gorilla"]
    }
  }
}
```

There is also `KV_SETQ`, `KV_ADD` and `KV_ADDQ`:

- `KV_SETQ` : the same as `KV_SET` but a response is only sent if the `KV_SETQ` fails
- `KV_ADD` : this returns an error if the key already exists (whereas `KV_SET` overwrites the value)
- `KV_ADDQ` : the same as `KV_ADD` but only sends a response if the `KV_ADDQ` fails (i.e. the key already exists)

<br/><br/>

# Store Multiple Values

```json
{
  "KV_SET":
  {
    "user123_forename":"James",
    "user123_surname":"Smith"
  }
}
```

<br/><br/>

# Set Response

`KV_SET` will respond with a `KV_SET_RSP` for *each* value stored. The response includes the original key and a status to indicate success or failure. 

If you don't need confirmation, you can use `KV_SETQ` (set quiet) which only sends a response on failure.


<br/><br/>

# Retrieve a Value
Use the `KV_GET` query which returns a `KV_GET_RSP`:

```json
{
  "KV_GET":["user123_forename"]
}
```

<br/>
Response:

```json
{
  "GET_RSP":
  {
    "user123_forename":"James"
  }
}
```

<br/>

# Retrieve Multiple Values
`KV_GET` is an array so you can retrieve multiple values in one call. Values are returned in **separate** responses:

```json
{
  "KV_GET":["user123_forename","user123_surname"]
}
```

This will return **two** `KV_GET_RSP` responses in an undefined order.