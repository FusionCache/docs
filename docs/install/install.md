---
layout: default
title: Download & Install
nav_order: 35
has_children: true
---

# Debian Package

Fusion is only available as a x86-64bit Debian package:

| Architecture | Version | |
|:---|:---:|:---:|
|x86 64-bit| 0.2.4 | [Download](https://fusion.gateway.scarf.sh/package/fusioncache_0.2.4_amd64.deb)|


{: .important}
> - There is not a Windows build but it may work in WSL2
> - Docker image will be published soon

<br/> 

# Install

Install with `dpkg`, for example for version 0.2.4:

`sudo dpkg -i fusioncache_0.2.4_amd64.deb`
 
This is installs to: `/usr/local/bin/fusioncache`

See [Run](run.md) for starting instructions.
