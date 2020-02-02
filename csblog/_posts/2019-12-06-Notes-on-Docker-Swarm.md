---
layout: post
title: "Notes on Docker Swarm"
categories:
  - notes
  - csblog
tags:
  - notes
---
## Notes
Swarm mode is built into Docker since 1.12. Information about the standalone docker swarm can be found [here](https://docs.docker.com/swarm/overview/)

Docker swarm allows you to expose the ports for services to an external load balancer as well as providers you a way to specify the distribution of service containers between nodes

With swarm services you can modify a service's configuration, including the networks and volumes it is connected to without manually restarting the service

Metadata labels for the nodes can be used for service and scheduling constraints. These labels are separate from the docker daemon labels.  

Scheduling constraints can be specified when creating a docker service. Look at [this CLI reference](https://docs.docker.com/engine/reference/commandline/service_create/) for more details.

You can configure services to start automatically when Docker starts.

When you create a service, the image’s tag is resolved to the specific digest the tag points to at the time of service creation. Worker nodes for that service use that specific digest forever unless the service is explicitly updated.

If swarm manager can not resolve an image with specific tag, worker node tries to do it.

Placement constraints are strict while placement preferences are best-effort

To reserve a given amount of memory or number of CPUs for a service, use the `--reserve-memory` or `--reserve-cpu` flags.

Placement preferences are processed in the order they are encountered.

See [these sections of the documentation](https://docs.docker.com/engine/swarm/#configure-a-services-update-behavior) for service updates, manual and automatic rollbacks (Docker 17.04 and higher).

Volumes: Bind mounts are file system paths from the host where the scheduler deploys the container for the task. Docker mounts the path into the container. Data volumes are storage that exist independently of a container. The lifecycle of data volumes under swarm services is similar to that under containers. Volumes outlive tasks and services, so their removal must be managed separately.

[Docker Configs](https://docs.docker.com/engine/swarm/configs/) can be used to store non-sensitive information such as configuration file outside a service's image or running containers. They can be added or removed from a service at any time and services can share a config. Files used for docker configs and secrets have to be less than 500KB.

[Docker Secret](https://docs.docker.com/engine/swarm/secrets/) can be used to sensitive data that should not be transmitted over a network or stored unencrypted in a Dockerfile or in your application code.

### Terminologies
**Node:** Instance of Docker engine participating in the swarm

**Service:** A service is the definition of the task to execute on the manager or worker nodes. Replicated services will have specific number of replica tasks among the nodes. Global services make sure that one task for the service runs on every available node in the cluster

**Task:** Task carries a docker container and the command to run inside the container. Once the task is assigned to a node, it can not move to another node (except when the node fails and the task is rescheduled)

**Ingress load balancing** exposes the services you want to make available externally to the swarm. See [this doc](https://docs.docker.com/engine/swarm/ingress/) for more info.

**PublishedPort:** External components can access the service on the PublishedPort of any node in the cluster (using a [routing mesh](https://docs.docker.com/engine/swarm/ingress/))

**Internal load balancing:** Swarm manager uses internal load balancing to redistribute requests among services using an internal DNS component

Routing mesh can be bypassed by setting the service to `host` mode

Docker swarm uses a Raft implementation to maintain a consistent internal state of the entire swarm for high availability settings. See more details [here](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/).

Even if a swarm loses the quorum of managers, swarm tasks on existing worker nodes continue to run. However, swarm nodes cannot be added, updated, or removed, and new or existing tasks cannot be started, stopped, moved, or updated.

[Docker plugins](https://docs.docker.com/engine/extend/plugin_api/)

## Tasks and scheduling
![Service creation and task scheduling](https://docs.docker.com/engine/swarm/images/service-lifecycle.png)

Dispatcher is the one responsible for assigning tasks to nodes


**References:**
