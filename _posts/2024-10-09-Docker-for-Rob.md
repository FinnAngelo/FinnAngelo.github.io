---
layout: post
title: "Docker for Rob"
categories: wsl docker chocolatey webtop
published: true
---

I've been working talking with my brother about how to setup Ollama the easy 
way... yep, it just needs docker.

But before we go there, lets setup docker 😱

----------------------------------------

## TL;DR

1. [What is docker anyways?](#what-is-docker-anyways)
2. [Backup your stuff](#backup-your-stuff)
3. [Open prompt as admin](#open-prompt-as-admin)
4. [Install Chocolatey](#install-chocolatey)
5. [Enable WSL2](#enable-wsl2)
6. [Install Docker Desktop](#install-docker-desktop)
7. The fun bit - [Test with WebTop!](#test-with-webtop)

----------------------------------------

## What is docker anyways

A really bad analogy is its like a virtual pc.

Have you ever run linux inside QEMU or Virtual PC and then deleted the image?

Its sort-of like that, but _usually_ without a pretty desktop. 

----------------------------------------

## Backup your stuff

We're about to install wierd crap onto your PC... have you backed up recently?

----------------------------------------

## Open prompt as admin

Everyone probably knows this, but this is the fastest way I know to get an 
admin prompt 

1. `win-r` to run
2. enter `cmd` (or `powershell` or `pwsh` to get a powershell terminal)
3. Ctrl+Shift-> enter so prompt runs as admin

-----------------------------------------

## Install Chocolatey

- <https://chocolatey.org/install>

Just cut/paste this into your prompt:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Make sure you close the prompt window to set ... stuff

----------------------------------------

## Enable WSL2

- <https://learn.microsoft.com/en-us/windows/wsl/install>

run this one as admin `powershell`

```powershell
wsl --version # is it installed?
wsl --install
wsl --version
```

There _may_ be pre-requisites listed in the link... but probably not.

----------------------------------------

## Install Docker Desktop

- <https://docs.docker.com/desktop/install/windows-install/>

There _might_ be other pre-reqs for docker from the link?

Anyways, run this one as admin `cmd`

```powershell
choco install docker-desktop -y
```

1. In your windows menu, open 'Docker Desktop', maybe pin it to the start.
2. in your system tray is a teeny-tiny whale. 
   - Right click it and go to the dashboard

----------------------------------------

## Test with WebTop

- <https://docs.linuxserver.io/images/docker-webtop>
- versions of linux instead of `latest`
  - <https://github.com/linuxserver/docker-webtop?tab=readme-ov-file#version-tags>

This is the fun bit!

1. `docker pull lscr.io/linuxserver/webtop:latest`
   - this is optional, but its nice to see it download
2. run this:  
   `docker run -d --name webtop -p 3000:3000 --restart unless-stopped lscr.io/linuxserver/webtop:latest`
3. browse at <http://localhost:3000>

Yep - its a freakin' desktop inside a container!

1. Goto the docker desktop dashboard > Containers
2. trashcan the `lscr.io/linuxserver/webtop` image
3. run the `docker run...etc` again.
   - it starts soooo much faster than a virtual pc!

----------------------------------------

## All you ever need to know about Docker

- <https://www.finnangelo.com/2019/09/05/Docker-Cheatsheet.html>

----------------------------------------

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
