---
layout: post
title: "Docker Cheatsheet"
published: true
---

The useful stuff I use with Docker  
(ToDo: Get picture)

----------------------------------------

+ [Setup](#Setup)
  + [Change drive of hyper-v disks](#Change-drive-of-hyper-v-disks)
+ [docker pull](#docker-pull)
+ [docker container](#docker-container)
+ [docker image](#docker-image)
+ [docker prune](#docker-prune)
+ [Credits](#Credits)    

## Setup ##

### Change drive of hyper-v disks ###

01. Stop docker
02. create `D:\Users\Public\Public Documents\Hyper-V\Virtual hard disks`
03. Cut/Paste docker hard disk `DockerDesktop.vhdx` from
`C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks`
04. Open Hyper-V manager > Select PC > Actions pane > Hyper-V Settings...
    + Virtual Hard Disks = `D:\Users\Public\Documents\Hyper-V\Virtual Hard Disks`
05. Restart Docker
06. Test with  
    ```powershell
    docker run -di --rm --name test mcr.microsoft.com/dotnet/framework/sdk
    ```

----------------------------------------

## docker pull ##

Good images to have:

```powershell
docker pull mcr.microsoft.com/dotnet/framework/sdk
docker pull mcr.microsoft.com/windows/servercore/iis
```

----------------------------------------

## docker container ##

<https://docs.docker.com/engine/reference/commandline/container/>

```powershell
docker container ls
```
----------------------------------------

## docker image ##

```powershell
docker image ls

docker image prune --all --force
```
----------------------------------------

## docker prune ##

<https://docs.docker.com/config/pruning/>

```powershell
docker container ls
docker volumes ls
docker image ls
docker network ls

docker container prune
docker volumes prune
docker image prune --all --force

docker network ls
# docker system prune --volumes #This is the nuclear option
```

----------------------------------------

## Credits ##

+ No names yet

_Cheers!_
