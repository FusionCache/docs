---
layout: default
title: Server Info
nav_order: 45
parent: KV API
has_children: false
---

# KV_SERVER_INFO
Requests server information.


<br/>


## Structure

An empty object:

```json
{
  "KV_SERVER_INFO":{}
}
```


<br/>

## Response
`KV_SERVER_INFO_RSP` object containing:

| Key | Information |
|:---|:---|
|`st`       | unsigned int: `1` (always successful)
|`qryCnt`   | unsigned int: number of queries since startup, including the `SERVER_INFO`|
|`version`  | string: Fusion version, in format `major.minor.revision`|


<br/>

## Example

```json
{
  "KV_SERVER_INFO_RSP":
  {
    "st": 1,
    "qryCnt": 12345,
    "version": "0.2.0"
  }
}
```