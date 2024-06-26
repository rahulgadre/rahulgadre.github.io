---
title: Docker 101 🐳
date: 2020-09-22T17:25:25.364Z
tags: ["Docker"] 
image: "https://raw.githubusercontent.com/rahulgadre/rahulgadre.github.io/main/content/blog/images/docker101.jpg"
description: Basic Docker Commands
---

Docker is a computer program that performs operating-system-level virtualization, also known as “containerization”. Docker allows to run multiple containers on a single linux OS. Containers are used for fast application deployment. Containers are running instances of an image . Unlike in virtualization using VMWare, containers have only one base operating system. Therefore, containers are widely used in the Dev-Ops environment for faster application deployment.

When a virtualized environment is created, there are a lot of resources that get allocated such as RAM, hard disk etc. When a virtual machine boots up, it may not consume all the resources that were allocated to it. Therefore, in this case many resources get wasted and that’s where containers can be more helpful. Containers use only the resources that are needed.

### Tidbits:

- Docker run - If directly used then it downloads an image from Docker Hub and then starts a container. Normally used to start containers.
- Docker build - Builds a container based on the commands written in the Dockerfile. Docker build is usually followed by Docker run to start containers using images.
- Docker compose - Starting multiple containers using Docker run command might be time consuming. Docker compose solves this problem. With Docker compose, a YAML file is written with the instructions to build multiple containers.
- Docker Swarm - Docker's orchestration tool. It contains manager nodes and worker nodes. Services can be created/updated/removed on the fly. Docker visualizer gives a graphical view of how containers being run in the Swarm cluster.
- Docker Stack - A stack is a group of interrelated services that share dependencies, and can be orchestrated and scaled together. A single stack is capable of defining and coordinating the functionality of an entire application (though very complex applications may want to use multiple stacks).


### Important Docker commands:

- List all containers (only IDs) : `docker ps -aq`
- Stop all running containers:   `docker stop $(docker ps -aq)`
- Remove all containers: `docker rm $(docker ps -aq)`
- Remove all images: `docker rmi $(docker images -q)`
- Create image using this directory’s Dockerfile : `docker build -t mycontainer` . -t is used to name a container.
- Run “mycontainer” mapping port 8080 to 80: `docker container run -p 8080:80 mycontainer`
- Map a volume to docker containers : `docker run -p 8080:80 -v /Desktop/docker/src:/var/www/html mycontainer`
- Run “mycontainer” mapping port 8080 to 80, but in background/detached mode: `docker run -d -p 8080:80 mycontainer`
- See a list of all running containers: `docker container ls`
- Gracefully stop the specified container: `docker container stop <hash>`
- See a list of all containers, even the ones not running: `docker container ls -a`
- Force shutdown of the specified container: `docker kill <hash>`
- Remove the specified container from this machine: `docker container rm <hash>`
- Show all images on this machine: `docker images -a`
- Log into a CLI session using your Docker credentials: `docker login`
- Tag <image> for upload to registry: `docker tag <image> username/repository:tag`
- Upload tagged image to registry: `docker push username/repository:tag`
- Run image from a registry: `docker run username/repository:tag`
- List Docker volume: `docker volume ls`
- List Docker Network: `docker network ls`

#### LEGACY COMMANDS:

- LEGACY: Remove the specified image from this machine: `docker rmi <imagename>`
- LEGACY:Remove all images from this machine: `docker rmi $(docker images -q)`
- LEGACY: Remove all images with dependencies: `docker images -q | xargs docker rmi –f`
