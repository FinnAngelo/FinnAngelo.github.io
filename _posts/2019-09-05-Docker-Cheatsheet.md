---
layout: post
title: "Docker Cheatsheet"
published: true
---

The useful stuff I use with Docker  
(ToDo: Get picture)

----------------------------------------

+ [Setup](#Setup)
  + [Change drive of hyper-v disks](#change-drive-of-hyper-v-disks)
  + [Change drive of docker images](#change-drive-of-docker-images]
+ [docker pull](#docker-pull)
+ [docker container](#docker-container)
+ [docker image](#docker-image)
+ [docker prune](#docker-prune)
+ [Credits](#credits)    

## Setup ##

### Change drive of hyper-v disks ###

This does slow performance as my `D:` drive isn't an SSD, but my `C:` drive is always low on space!

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

### Change drive of docker images ###

<https://www.pbworks.net/change-docker-images-location-in-windows/>

01. Stop docker
02. Purge all the images, because is messy to try move them
03. Create `D:\ProgramData\Docker`
04. Edit `C:\ProgramData\Docker\config\daemon.json`
05. Add `"graph": "D:\\ProgramData\\Docker"`
06. Restart docker
07. Test with 
    ```powershell
    docker pull mcr.microsoft.com/dotnet/framework/sdk
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
