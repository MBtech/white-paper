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

Removing a node from node list after the node has left: `docker node rm <node_id>`

Scaling a service: `docker service ps <service_ID>=<number_of_tasks>`

Delete a service: `docker service rm <service_ID>`

Manager only node: `docker node update --availability drain <NODE>`

Adding a label to a node: `docker node update --label-add foo --label-add bar=baz node-1`

Specifying placement constraints:
```bash
docker service create \
  --name my-nginx \
  --replicas 5 \
  --constraint node.labels.region==east \
  nginx
```
You can also use the `constraint` service-level key in a `docker-compose.yml` file.

Placement Preferences:
```bash
docker service create \
  --replicas 9 \
  --name redis_2 \
  --placement-pref 'spread=node.labels.datacenter' \
  redis:3.0.6
```

Rolling updates: See the details [here](https://docs.docker.com/engine/swarm/swarm-tutorial/rolling-update/)

Drain a node: You can drain a node to allow docker swarm to move the services to another node e.g. for maintenance of a node. See the [guide](https://docs.docker.com/engine/swarm/swarm-tutorial/drain-node/) for details.

Creating service using image on a private registry: Use [this guide](https://docs.docker.com/engine/swarm/#create-a-service-using-an-image-on-a-private-registry)

Configuring runtime environment of a container: Use [this guide](https://docs.docker.com/engine/swarm/#configure-the-runtime-environment)

Creating overlay network for docker swarm: Use [this guide](https://docs.docker.com/engine/swarm/#connect-the-service-to-an-overlay-network)

**References:**
