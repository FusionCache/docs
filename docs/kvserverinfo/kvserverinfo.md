---
layout: default
title: Server Info
nav_order: 6
parent: KV API
has_children: false
---

# Server Info
Requests server information.


<br/>


## Structure

An empty object:

```json
{
  "SERVER_INFO":
  {    
  }
}
```


<br/>

## Response
`SERVER_INFO_RSP` object containing the status (`st`) which is always `0` (Success) and :

| Key | Information |
|:---|:---|
|`keyCnt`   | unsigned int: number of keys cached |
|`qryCnt`   | unsigned int: number of queries since startup|
|`version`  | string: Fusion version, in format `major.minor.revision`|


<br/>

## Example

```json
{
  "SERVER_INFO_RSP":
  {
    "st": 0,
    "keyCnt": 5632,
    "qryCnt": 928,
    "version": "0.1.5"
  }
}
```