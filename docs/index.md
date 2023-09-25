---
layout: default
title: Overview
nav_order: 1
has_children: false
---


# Overview

FusionCache is a JSON cache, handling as key-values. It is to Redis and memcached, except the queries are JSON based over WebSockets.

<br/>

[KeyValue](keyvalues.md) has more information and there's a [quick start](https://www.fusioncache.io/quick-start/) guide.

<br/>

## Start Here
Fusion is available as a Debian package.

{: .important}
> Fusion is only available for 64bit x86 CPUs. An ARM build will be available in the future.
>
> It has not been tested on Mac or Windows, although it should run on WSL2 in Windows.
>


<br/>

## Limitations
Fusion is still in alpha and has limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Threads| There is a 64 thread limit. This is not a technical limitation, it will increase as development progresses.|
|Memory| The cache has no data eviction. <br/> Of course you can delete data at any time.<br/> A future release will address this. |
|Security| The interfaces use HTTP rather than HTTPS. Fusion is not intended for a public network.

