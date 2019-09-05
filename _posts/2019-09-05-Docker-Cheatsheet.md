---
layout: post
title: "Docker Cheatsheet"
published: true
---

The useful stuff I use with Docker  
(ToDo: Get picture)

----------------------------------------

+ [docker pull](#docker-pull)
+ [docker container](#docker-container)
+ [docker image](#docker-image)
+ [docker prune](#docker-prune)
+ [Credits](#Credits)    

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
