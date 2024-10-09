---
layout: post
title: "My Docker Cheatsheet"
published: true
---

The useful stuff I use with Docker  
(ToDo: Get picture)

**UPDATE: The install process has probably been updated; using the commands is probably the same though**
----------------------------------------

+ [Install](#install)
+ [docker pull](#docker-pull)
+ [docker container](#docker-container)
+ [docker image](#docker-image)
+ [docker prune](#docker-prune)
+ [docker volume](#docker-volume)
+ [docker run](#docker-run)
+ [webtop](#webtop)
+ [mssql-server-windows-developer](#mssql-server-windows-developer)
+ [Credits](#credits)    

----------------------------------------

## Install ##

**Update: I had a bunch on hyper-v here, but it was quite out of date**

### Enable WSL2 ###

- https://learn.microsoft.com/en-us/windows/wsl/install

run this one as admin `powershell`

```powershell
wsl --version # is it installed?
wsl --install
wsl --version
```

### Install chocolatey ###

- https://chocolatey.org/install

### Install Docker ###

- https://docs.docker.com/desktop/install/windows-install/

```powershell
choco install docker-desktop -y
```

----------------------------------------

## docker pull ##

Good images to have:

```powershell
docker pull mcr.microsoft.com/dotnet/framework/sdk
docker pull mcr.microsoft.com/windows/servercore/iis
docker pull lscr.io/linuxserver/webtop:latest
```

----------------------------------------

## docker container ##

<https://docs.docker.com/engine/reference/commandline/container/>

```powershell
docker container ls

docker ps # Lists running containers
docker kill ab1234cd # Kills container - useful with the -rm tag on the running container!
docker ps -a # Lists all containers

docker rm ab123cd # removes container
```
----------------------------------------

## docker image ##

<https://docs.docker.com/engine/reference/commandline/image/>

```powershell
docker image ls
docker images
docker image rm IMAGE
docker rmi ab123cd # remove image

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

## docker volume ##

<https://docs.docker.com/engine/reference/commandline/volume/>

```powershell
docker run --help
docker volume ls
docker volume inspect
docker volume prune
docker volume rm
```
----------------------------------------

## docker run ##

<https://docs.docker.com/engine/reference/commandline/run/>

```powershell
docker run --help 
```

| Parameter               | Note
|-------------------------|---------------------------------------------------
| -d, --detach            | Run container in background and print container ID
| -u, --user 0            | Username or UID - 0 is for root
| -p, --publish 5901:5901 | Publish a container's port(s) to the host
| -e, --env list VNC_RESOLUTION=1900x1175       | Set environment variables
| --rm                    | Automatically remove the container when it exits
| --name testme           | Assign a name to the container
| -v, --volume /D/TempOnHost:/C/TempOnContainer |
| -i, --interactive       | need -it to keep the docker container running! IIS doesn't need it
| -t, --tty               | Allocate a pseudo-TTY, -it to use putty shell 

----------------------------------------

## webtop ##

This is fun!

- https://docs.linuxserver.io/images/docker-webtop
- versions of linux instead of `latest`
  - https://github.com/linuxserver/docker-webtop?tab=readme-ov-file#version-tags

To run

1. `docker pull lscr.io/linuxserver/webtop:latest`
   - this is optional, but its nice to see it download
2. run this:  
   `docker run -d --name webtop -p 3000:3000 --restart unless-stopped lscr.io/linuxserver/webtop:latest`
3. browse at <http://localhost:3000>

----------------------------------------

## mssql-server-windows-developer ##

I can never remember this, and it is really handy!  
It looks like the image is at least 2 years old, but I'm using it anyways because I live life on the edge...

[Octopus - Running SQL Server Developer in a Windows-based Docker Container](https://octopus.com/blog/running-sql-server-developer-install-with-docker)

Here is the run goodness:

```powershell
docker pull microsoft/mssql-server-windows-developer
docker run --rm --name SQLServer -d -p 1433:1433 -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer
```

And we can just use Sql Server Management Studio or [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15)... or heck [Database Browser Portable](https://portableapps.com/apps/development/database_browser_portable) to connect

## Credits ##

+ No names yet

_Cheers!_
