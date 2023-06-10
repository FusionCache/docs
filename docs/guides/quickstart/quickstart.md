---
layout: default
title: Quickstart
parent: Guides
nav_order: 1
has_children: true
---

# Quick Start
This guide takes you from nothing to sending queries to Fusion:

1. Install
2. Run
3. Store objects
4. Query objects

The guide uses Postman to query the server, which provides a cross-platform UI to create HTTP clients, including WebSocket clients.

Postman is required from step 3, it can be downloaded from [here](https://www.postman.com/downloads/).

It also a good idea to read [Concepts](../../concepts.md) before continuing.

<br/>

# Install
Follow the [Install](../../install/install.md) instructions to download and install the Debian package.

<br/>

# Run
The [Run](../../install/run.md) contains full instructions but for this guide we can start with Rest and WS Standard interfaces on localhost, default buffer sizes and ports.

The default ports are 1981 and 1982.

If these ports are used by other software, you can use `--restQueryPort` or `--wsQueryPort`.

1. Enter the install directory: `cd /usr/local/bin/fusioncache`
2. Start: `./fusionserver --restQueryIp=127.0.0.1 --wsQueryIp=127.0.0.1`

The server will report similar to:

```console
Fusion v0.1.0 starting
Checking startup arguments
Registering signals
Available threads: 16
I/O Threads: 2
Normal: 1
Rest: 1
Starting WebSocket Normal server 127.0.0.1:1982
Starting Query Rest server on 127.0.0.1:1981
Started WebSocket Normal server on 127.0.0.1:1982
Started Query Rest server on 127.0.0.1:1981
Fusion ready
```
<br/>

# Query

## Server Info

1. Open Postman and create a new HTTP `GET` request
2. Ensure the request is `GET` and the address is `localhost:1981`
3. Select `Body` tab, just under the address bar
4. Inside `Body` tab, enter:
```json
{
  "SERVER_INFO":
  {    
  }
}
```

![Postman](quickstart_1_serverinfo.png "HTTP Get Request")

5. Press Send and Fusion will return server information:

![Postman](quickstart_2_serverinfo_rsp.png "Server Info Response")

<br/>


## Create Class

1. In Postman, replace the `SERVER_INFO` query with the following and press Send:

```json
{
  "CREATE_CLASSES":
  {    
    "Address":
    {
      "city":"string"
    },
    "Person":
    {
      "forename":"string",
      "surname":"string",
      "address":"Address"
    }
  }
}
```

2. This creates an `Address` and `Person` class. The `Person` class has an `address` member which type `Address`. The response has the two classes without errors:
```json
{
  "CREATE_CLASSES_RSP":
  {
    "Address": {},
    "Person": {}
  }
}
```
<br/>

## Store Person and Address Objects

1. Replace the `CREATE_CLASSES` query with:

```json
{
  "STORE":
  { 
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"John",
        "surname":"Wick",
        "address":
        {
          "city":"New York"
        }
      }
    ]    
  }
}
```
2. This creates two objects - a `Person` and an `Address` object
3. The response will be similar to this but with different `_oid` values:
```json
{
  "STORE_RSP": [
    {
      "Person": {
        "address": {
          "Address": {
            "_oid": "6201f873-67c1-495b-a20a-23356b401635"
          }
        },
        "_oid": "a1eca163-7d2f-40d0-a6fc-ba717ba19930"
      }
    }
  ]
}
```

## Get Object
To get the objects from the cache, we use `GET`, which requires an OID, which we have in the `STORE_RSP`. 

The easiest way is to use the "Duplicate Tab" feature in Postman. Click the three dots and click "Duplicate selected tab":

![Postman](quickstart_3_duplicatetab.png "Duplicate tab")

1. In the new tab, select "Body". In the following query, you must set the OID in `_oids` to the `Person::_oid` returned in the `STORE_RSP` above. For this example, we do this:

```json
{
  "GET":
  { 
    "Person":
    {
      "_oids":["ce614727-5ea3-449c-8b9d-62716f94d13c"]
    }
  }
}
```
2. Press Send and the response will be as below but with different OIDs:
```json
{
  "GET_RSP":
  [
    {
      "forename": "John",
      "surname": "Wick",
      "address":
      {
        "Address":
        {
          "city": "New York",
          "_oid": "6201f873-67c1-495b-a20a-23356b401635"
        }
      },
      "_oid": "a1eca163-7d2f-40d0-a6fc-ba717ba19930"
    }
  ]
}
```

This shows how Fusion manages relationships between objects: the `Person` class contains the `address` member, and because it manages object relationships, it can return the appropriate `Address` object.

<br/>

### Store More Objects

1. We'll store three more `Person` objects by replacing the first `STORE` with:
```json
{
  "STORE":
  { 
    "_class":"Person",
    "_objects":
    [
      {
        "forename":"James",
        "surname":"Bond",
        "address":
        {
          "city":"London"
        }
      },
      {
        "forename":"Jason",
        "surname":"Bourne",
        "address":
        {
          "city":"Paris"
        }
      },
      {
        "forename":"The",
        "surname":"Rock",
        "address":
        {
          "city":"Paris"
        }
      }
    ]
  }
}
```
2. The response will show the OIDs for the three `Person` and three `Address` objects 


### Find
The `STORE` queries put The Rock and Jason Bourne in Paris, let's confirm that by searching the cache with `FIND`.

1. In the tab that contains the `GET` query, replace the query with:
```json
{
  "FIND":
  { 
    "Person":
    {
      "address":
      {
        "city":"Paris"
      }
    }
  }
}
```
2. This says, "Return the `Person` objects with `Person::address::city` equal to "Paris". The `FIND_RSP` response has the `Person` objects for Jason Bourne and The Rock.
