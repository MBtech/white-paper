---
layout: post
title: "Tutorial: Docker Swarm"
pagination: 
  enabled: true
  category: csblog
categories:
  - tutorial
  - csblog
tags:
  - tutorial
comments: true
---

## How to use run a Docker Swarm

***Note: This tutorial is heavily based on the documentation at Docker's own website and the manual Swarm creation [guide](https://docs.docker.com/swarm/install-manual/) by Docker. I have summarized it and added a couple of extra things that I found out while trying it out myself.***  

Docker Inc. recently released a native Docker clustering mechanism which turns several docker hosts into a single Virtual docker host. Thus allow you to run you docker containers across several hosts. It also lets you choose the constraints and different scheduling techniques for scheduling the docker containers onto different hosts in the Swarm.

Following are the steps necessary to get started with Docker Swarm on a set of Linux Hosts:

### Step 1: Installing Docker Engine on hosts

First of all, we need to install docker engine on all the machines  
`$ curl -sSL https://get.docker.com/ | sh`  

Start the docker daemon  
`$ sudo docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock &`  
This will make Docker daemon listen for local connections as well as external connections on any external interface. Note that this method is not secure by default as anyone with access to that IP and port will have a full Docker access. Therefore a secure proxy is advisable to limit the access. Take a look at this [post](https://docs.docker.com/engine/security/https/) for guidance regarding that.

_If the docker daemon is running (with the default options) already, you will get an error like this if you execute the previous command:_
`FATA[0000] Error starting daemon: pid file found, ensure docker is not running or delete /var/run/docker.pid`  
You can stop the docker daemon using:  
`sudo service docker stop`

Test the docker installation  
`$ sudo docker run hello-world`  

If you want to run docker as a non-root user, you need to add the user to the docker group  
`$ sudo usermod -aG docker ubuntu`  
Replace *ubuntu* with your username  
This only take effects after logging out   
`$ logout`

### Step 2: Setup Discovery Backend

Log into the node that you want to host a backend discovery service container and run:  
`$ docker run -d -p 8500:8500 --name=consul progrium/consul -server -bootstrap`

### Step 3: Create Swarm Cluster

Now log into the node you want Swarm manager to run on and run:  
`docker run -d -p 4000:4000 swarm manage -H :4000 --replication --advertise <manager_ip>:4000 consul://<consul_ip>`  
You can also run multiple Swarm manager to fault tolerance. Simply run the previous command in another node with that node's IP and this new Swarm manager will become the secondary Swarm manager that will take over if the primary one fails.

Lastly, run the following command on the nodes that you want to join the Swarm Cluster:  
`docker run -d swarm join --advertise=<node_ip>:2375 consul://<consul_ip>:8500`

You can use `docker ps` to see if a particular node is running the required container or not. Containers that terminate after the execution is done might not show up in when you list the running containers.

### Step 4: Communicate with Swarm Cluster

Use `docker -H :4000 info` on the Swarm manager node to see the nodes that are in the Swarm and their respective information.

Run a container using:  
`$ docker -H :4000 run hello-world`

and use
`docker -H :4000 ps` to see where is has been placed.

Let me know if something doesn't work for you or if you have any question.
