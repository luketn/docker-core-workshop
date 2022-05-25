# Docker Core Workshop

## Setup
If you don't already have it, follow instructions to install Docker for Desktop:
https://docs.docker.com/desktop/mac/install/
You'll know it's working if you can run the following on the command line:  
```
docker run hello-world
```

And see a successful result:
```
Hello from Docker!
```

This message shows that your installation appears to be working correctly.

You should also clone this repository to your local machine:
```shell
git clone https://github.com/luketn/docker-core-workshop.git
```

## Overview
Docker is a containerization technology. It allows you to package an executable process and associated files in a container image, and then run that program on a host which can share its Linux kernel, memory, network, disk resources with the process. The host can execute one or more of these containerised processes in parallel and allocate an isolated subset of its resources to each.

There are a few parts to it:

### Docker Image
A docker image is a really simple but beautiful thing. From the spec:  
> "An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime."  
https://github.com/moby/moby/blob/master/image/spec/v1.md

This layering of changes to the file system is a key innovation, and allows for each layer to be separately cached and shared between containers.

### Docker Daemon
The Docker daemon is a program which runs on the host machine and manages the containers. It is responsible for creating, starting, stopping, and removing containers. 

### Docker CLI
The Docker CLI allows you to for managing the images which are used to create containers.

### Further Reading
Excellent video on the fundamentals of Docker:  
https://docs.docker.com/get-started/#what-is-a-container  
Some history:  
https://www.baeldung.com/linux/docker-containers-evolution


## Workshop
Enough chit-chat - let's build something!

### Part 1 - Docker FROM scratch
Go [here](1-docker-from-scratch/README.md) to build a bare-bones docker container image.

### Part 2 - Java in Docker
Let's jump to the other end of the pool and build something bigger. 
Go [here](2-java-in-docker/README.md) to build a Java microservice in Docker.
