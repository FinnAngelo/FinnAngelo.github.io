---
layout: post
title: "My Docker Cheatsheet"
published: true
---

The useful stuff I use with Docker  
(ToDo: Get picture)

----------------------------------------

+ [Setup](#setup)
  + [Install](#install)
  + [Change drive of hyper-v disks](#change-drive-of-hyper-v-disks)
  + [Change drive of docker images](#change-drive-of-docker-images)
+ [docker pull](#docker-pull)
+ [docker container](#docker-container)
+ [docker image](#docker-image)
+ [docker prune](#docker-prune)
+ [docker volume](#docker-volume)
+ [docker run](#docker-run)
+ [mssql-server-windows-developer](#mssql-server-windows-developer)
+ [Credits](#credits)    

----------------------------------------

## Setup ##

### Install ###

Install Hyper-V - may require restart

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName:Microsoft-Hyper-V -All
```

Install chocolatey:  
<https://chocolatey.org/install>

Install Docker

```powershell
choco install docker-desktop -y
```

### Change drive of hyper-v disks ###

This is easier if you do it before installing docker so you don't have to move the `DockerDesktop.vhdx` hard disk

Changing the drive does slow performance as my `D:` drive isn't an SSD, but my `C:` drive is always low on space!

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

Do this before you download the first image, evem before the test above

01. Purge all the images, because is messy to try move them
02. Stop docker
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
