---
layout: post
title:  "Pre-Installation for Vector Tile Workshop"
categories:
  - Vector Tiles
  - Markup
tags:
  - Post Formats
  - readability
  - standard
last_modified_at: 2018-04-13
---


Following installation is required before the workshop.

1. Create GitHub account, [https://github.com](https://github.com).
- Please go [here for how-to](https://services.github.com/on-demand/intro-to-github/create-github-account). We'll use GitHub account to share webmaps we will build at the workshop.
2. Create Tilehosting account.
- Please go [here](https://admin.tilehosting.com/auth/widget?mode=select) to create account. Tilehosting will provide vector basemaps for web map.
3. In order to install Docker and other software during the workshop, the user needs to have Administrative level privilege.
4. Please download Docker Community Edition according to one's own Operating System.
Please go [here](https://www.docker.com/community-edition) for your Operating System. For Windows 10 Home Edition, pleaes follow steps below for
<!--more-->

## Install Docker on MacOS
*  Docker for MacOS [Here](https://store.docker.com/editions/community/docker-ce-desktop-mac)
	1. Mount Docker.dmg
	2. Drag Docker App onto Applications Shortcut
	3. Launch Docker
	4. Agree to run application from Internet
	5. Supply admin password

## Install Docker on Windows 10 for Pro and Home Edition
* Docker for Windows [Here](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)
	1. __Requires Windows 10 Pro or Enterprise__
   2. 	Ensure that hardware virtualization support is turned on in the BIOS settings
* __Does Not Work with Windows 10 Home Edition__
* Approaches to work arround this:
	1.	Can be installed on [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
        + Can be installed on a virtual machine running linux
        + [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/)
            + Accept Defaults during install
            + Run the Quick Start Terminal
            + Execute the steps at the *Quick Start Terminal*
