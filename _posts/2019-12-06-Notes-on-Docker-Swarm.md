---
layout: post
title: "Notes on Docker Swarm"
categories:
  - notes
tags:
  - notes
---
## Notes
Swarm mode is built into Docker since 1.12. Information about the standalone docker swarm can be found [here](https://docs.docker.com/swarm/overview/)

Docker swarm allows you to expose the ports for services to an external load balancer as well as providers you a way to specify the distribution of service containers between nodes

With swarm services you can modify a service's configuration, including the networks and volumes it is connected to without manually restarting the service

### Terminologies
**Node:** Instance of Docker engine participating in the swarm
**Service:** A service is the definition of the task to execute on the manager or worker nodes. Replicated services will have specific number of replica tasks among the nodes. Global services make sure that one task for the service runs on every available node in the cluster
**Task:** Task carries a docker container and the command to run inside the container. Once the task is assigned to a node, it can not move to another node (except when the node fails and the task is rescheduled)
**Ingress load balancing** exposes the services you want to make available externally to the swarm. See [this doc](https://docs.docker.com/engine/swarm/ingress/) for more info.
**PublishedPort:** External components can access the service on the PublishedPort of any node in the cluster (using a [routing mesh](https://docs.docker.com/engine/swarm/ingress/))
**Internal load balancing:** Swarm manager uses internal load balancing to redistribute requests among services using an internal DNS component

Routing mesh can be bypassed by setting the service to `host` mode

Docker swarm uses a Raft implementation to maintain a consistent internal state of the entire swarm for high availability settings. See more details [here](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/).



**References:**
