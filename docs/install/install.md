---
layout: default
title: Install
nav_order: 35
has_children: true
---

# Install
Fusion is available as a Debian package. Docker images will be published soon.

<br/>

{: .important}
>- There is not an installer for Windows and there are no plans for this
>- The Debian package should run in WSL2 but this has not been tested
>- There is only a 64-bit build and no plans for a 32-bit build
>- An ARM build will be available later

<br/>


Fusion can be installed by downloading the Debian package or cloning the release repo.

<br/>

## Download

- [Intel/AMD 64-bit](https://fusion.gateway.scarf.sh/package/fusioncache_0.1.10_amd64.deb)

<br/> 

## Install

Install with `dpkg`, for example for version 0.1.11:

`sudo dpkg -i fusioncache_0.1.11_amd64.deb`
 
This is installs to: `/usr/local/bin/fusioncache`

See [Run](run.md) for starting instructions.










