---
layout: default
title: Install
nav_order: 4
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
There is only a 64-bit build and no plans for a 32-bit build.

An ARM build will be available later.

<br/>

{: .warning}
>Fusion is alpha, so no gaurantees are made and there may be breaking changes ahead.

<br/>

| Version     | Link        |
|:---|:---|
|0.1.0 alpha|[Download](https://github.com/FusionCache/releases/blob/main/0.1/fusioncache_0.1_amd64.deb)|


## Run
The package installs the server to `/usr/local/bin/fusioncache`, in which you'll find `fusionserver`.

See [Run](run.md) for starting instructions.










