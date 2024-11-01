---
layout: post
title: "Running Ollama with Docker"
categories: Ollama Docker Powershell
published: true
---

OKs, this is a quick post...

You have docker installed and now you want Ollama installed.  
Its very complex... read on!

----------------------------------------

## TLDR;

1. [Run Ollama in docker](#run-ollama-in-docker)
2. [Pull the models for Ollama](#pull-the-models-for-ollama)
3. [Test with localhost](#test-with-localhost)

----------------------------------------

## Run Ollama in docker

- <https://ollama.com/blog/ollama-is-now-available-as-an-official-docker-image>
- <https://hub.docker.com/r/ollama/ollama>

side note: Docker and Volumes - I can never remember this
- <https://forums.docker.com/t/whats-the-correct-way-to-mount-a-volume-on-docker-for-windows/58494/4>

Powershell prompt

- **this is using the powershell prompt for the local volume location!**
  - docker desktop has a terminal... but its nicer to use my own
- `${pwd}` is the folder that the command is running from
  - i.e. I'm running this prompt from `c:\GIT\deno-ollama` so the windows folder
    will be `c:\GIT\deno-ollama\ollama` while the linux one inside the container
    will be `root/.ollama`
    - docker-desktop lets you browse the contents under container actions

Install

1. ```powershell
   docker pull ollama/ollama:latest
   mkdir ollama
   docker run -d -v ${pwd}\ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:latest
   ```
   - note the docker image is running detached

----------------------------------------

## Pull the models for Ollama

Models

- <https://ollama.com/library>
- i.e.
  - dolphin-mistral
  - llama3
  - phi3.5

Running one:

1. ```powershell
   docker exec -it ollama ollama run phi3.5
   ```

----------------------------------------

## Test with localhost

1. goto <http://localhost:11434/>
   - you can check this on docker desktop
2. it shows:  
   `Ollama is running`
3. Yay!

----------------------------------------

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
