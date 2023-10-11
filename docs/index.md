---
layout: default
title: Overview
nav_order: 1
has_children: false
---


# Overview

FusionCache is a JSON cache, handling data as key-values. It is similar to Redis and memcached, except the querying is JSON based over WebSockets.

[KeyValue](keyvalues.md) has more information and there's a [quick start](https://www.fusioncache.io/quick-start/) guide.

<br/>

## Start Here
Fusion is available as a Debian package.

{: .important}
> Fusion is only available for 64bit x86 CPUs. An ARM build will be available in the future.
>
> It has not been tested on Mac or Windows, although it may run on WSL2.
>


<br/>

## Limitations
Fusion is new software and has limitations:


| Limitation            | Description               |
|:----------------------|:--------------------------|
|Cores| This version is limited to 8 cores. If your CPU has more than 8 cores, Fusion will run as if on 8 cores.|
|Memory| The cache has no data eviction. <br/> Of course you can delete data at any time.<br/> A future release will address this. |
|Security| The interfaces use HTTP rather than HTTPS. Fusion is not intended for a public network.

