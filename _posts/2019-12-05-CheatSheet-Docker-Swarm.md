---
layout: post
title: "CheatSheet for Docker Swarm"
categories:
  - cheatsheet
tags:
  - cheatsheet
---

Start docker swarm: `docker swarm init`

Status of docker nodes in the swarm: `docker node ls`

Deploying a service: `docker service create --replicas 1 --name helloworld alpine ping docker.com` starts a service named hello world with alphine linux container and runs command `ping docker.com`

Running services: `docker service ls`

Inspect service details: `docker service inspect --pretty <service_name>`

Which nodes are running a service: `docker service ps <service_ID>`

Scaling a service: `docker service ps <service_ID>=<number_of_tasks>`

Delete a service: `docker service rm <service_ID>`

Rolling updates: See the details [here](https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/)

Drain a node: You can drain a node to allow docker swarm to move the services to another node e.g. for maintenance of a node. See the [guide](https://docs.docker.com/engine/swarm/swarm-tutorial/drain-node/) for details.

**References:**
