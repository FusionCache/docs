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
>There is not an installer for Windows and there are no plans for this.
>The Debian package should run in WSL2 but this has not been tested.


<br/>

## Debian Package
- There is only a 64-bit build and no plans for a 32-bit build
- An ARM build will be available later.

<br/>

{: .warning}
>Fusion is alpha, so there are no gaurantees and there may be breaking changes ahead.

<br/>

## Download or Clone


- Go to [GitHub](https://github.com/FusionCache/releases/tree/main)
- Right-click on the `.deb` file and `Save link as...` or clone

<br/> 

## Install

Install with `dpkg`, for example for version 0.1.9:

`sudo dpkg -i fusioncache_0.1.9_amd64.deb`
 
This is installs to: `/usr/local/bin/fusioncache`

See [Run](run.md) for starting instructions.










