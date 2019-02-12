---
layout: post
title: "Tutorial: Getting Started with Docker"
categories:
  - tutorial
tags:
  - tutorial
comments: true
---

# Introduction to Docker and Useful commands

## What is docker
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.


### Useful commands
Installing docker  
`$ curl -sSL https://get.docker.com/ | sh`  

Download an image
`$ docker pull ubuntu`

Run an interactive container using ubuntu image with bash  
`docker run -i -t ubuntu /bin/bash`

See docker images
`docker images`

All docker containers (running or stopped)
`docker ps -a`

**Long Running containers**  
Start a very useful long-running process
`$ JOB=$(docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")`

Collect the output of the job so far
`$ docker logs $JOB`

Kill the job
`$ docker kill $JOB`

Stop the container
`$ docker stop $JOB`

Start the container
`$ docker start $JOB`

Restart the container
`$ docker restart $JOB`

Container must be stopped to remove it
`$ docker rm $JOB`

**Bind a service on TCP port**  
Bind port 4444 of this container, and tell netcat to listen on it
`$ JOB=$(docker run -d -p 4444 ubuntu:12.10 /bin/nc -l 4444)`

Which public port is NATed to my container?
`$ PORT=$(docker port $JOB 4444 | awk -F: '{ print $2 }')`

Connect to the public port
`$ echo hello world | nc 127.0.0.1 $PORT`

Verify that the network connection worked
`$ echo "Daemon received: $(docker logs $JOB)"`

**Dettaching from a container and reattaching**  
You can use the default escape sequence `Ctrl+p Ctrl+q` to dettach from a container.   
Reattaching can be done by using the docker attach command like:  
`docker attach ContainerID_or_Name`

[Docker daemon](https://docs.docker.com/engine/reference/commandline/daemon/)

### Problems:
#### Mac OSX
Problems with restarting docker daemon?
Run  
`$ docker-machine restart default`  
after which it recommended to  
`$ docker-machine env default`  
and  configure shell for the default machine  
`$ eval $(docker-machine env default)`  

The default machine configuration is available under the directory:
`~/.docker/machine/machines/default/`

### References:
[Docker Quick Start](https://docs.docker.com/engine/quickstart/)  
[Mac Installation Docker](https://docs.docker.com/v1.8/installation/mac/)
