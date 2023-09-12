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

## Download

- Go to [GitHub](https://github.com/FusionCache/releases/)
- Click on the .deb file then click the download file icon on the far right:
![Download icon](https://user-images.githubusercontent.com/129124415/267442177-c7fdba54-5a22-471d-bb85-7406ab2ad666.png)


## Clone

`git clone https://github.com/FusionCache/releases.git`


<br/> 

## Install

Install with `dpkg`, for example for version 0.1.9:

`sudo dpkg -i fusioncache_0.1.9_amd64.deb`
 
This is installs to: `/usr/local/bin/fusioncache`

See [Run](run.md) for starting instructions.










