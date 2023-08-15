---
layout: default
title: Key Values
parent: Concepts
nav_order: 1
---

# Key Values
This is associating (i.e. mapping) a unique key to a value. For example, we could associate `"user123_email"` with `"james123@email.com"` to store the email address for "user123".


Fusion will store the relationship between the key and its value so it can be retrieved later.

<br/>

# Values
A value can be: string, number, boolean, object or an array:

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

Objects and arrays can contain: string, number, boolean, object or array:

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
Key-value pairs are stored using the `SET` command:

```json
{
  "SET":
  {
    "user123_surname":"Smith"
  }
}
```
<br/>

```json
{
  "SET":
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

<br/><br/>

# Store Multiple Values

```json
{
  "SET":
  {
    "user123_forename":"James",
    "user123_surname":"Smith"
  }
}
```

<br/><br/>

# Set Response

`SET` will respond with a `SET_RSP` for *each* value stored. The response includes the original key and a status to indicate success or failure. 

If you don't need confirmation, you can use `SETQ` (set quiet) which only sends a response on failure.


<br/><br/>

# Retrieve a Value
Use the `GET` query which returns a `GET_RSP`:

```json
{
  "GET":
  [
    "user123_forename"
  ]
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
`GET` is an array so you can retrieve multiple values in one call. Values are returned in **separate** responses:

```json
{
  "GET":
  [
    "user123_forename",
    "user123_surname"
  ]
}
```

This will return **two** `GET_RSP` responses.