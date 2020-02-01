---
layout: post
title: "Notes on OpenFaaS"
categories:
  - notes
  - csblog
tags:
  - notes
---
## Notes
These are just a work-in-progress notes that I have from reading and experimenting with OpenFaaS

OpenFaaS has what they call a [PLONK stack](https://www.openfaas.com/blog/plonk-stack/):
- Prometheus: Metrics and time-series
- Linkerd (Optional) Service mesh
- OpenFaaS: Management and auto-scaling of compute
- NATS: Asynchronous message bus/queue
-, Kubernetes: Declarative, extensible, scale-out, self-healing clustering

From [their blogpost](https://www.openfaas.com/blog/plonk-stack/)
    The key difference between software like Docker Swarm and Kubernetes vs Docker containers, is that the former is declarative and the later is used imperatively. A declarative system says: “I want this, can you go off and do it for me?” and an imperative system says “Do exactly this, right now”.

NATS Streaming is used when functions are called asynchronously.

Backend provider interface for OpenFaaS can be implemented using [faas-provider](https://github.com/openfaas/faas-provider) SDK

OpenFaaS watchdog is responsible for starting and monitoring functions in OpenFaaS.

[of-watchdog](https://github.com/openfaas-incubator/of-watchdog/blob/master/README.md) enables the re-use of expensive resources such as database connection pools or machine-learning models. It provides the ability to keep function process warm between invocations.

OpenFaaS has it's own Autoscaling component based on alerts but one can also use the Kubernetes HPA when using a kubernetes deployment

There was some proposal related to provider agnostic constraints and limits, however that has been close. [Issue 479](https://github.com/openfaas/faas/issues/479)

You can use something called a stack file YAML to specify the functions that you want to deploy. Examples of this file can be seen in [lab3](https://github.com/openfaas/workshop/blob/master/lab3.md) of openfaas workshop. Further details about the YAML stack file can be seen in [these docs](https://docs.openfaas.com/reference/yaml/).

Function composition can be done through workflows. See [this lab](https://github.com/openfaas/workshop/blob/master/lab4.md#create-workflows)

If you just edit the .yaml file then you don't need to build the function. You can simply use `faas-cli deploy` instead.

The following services are part of the openfaas stack (in Docker swarm):
- func_queue-worker: NATS Streaming queue

[OpenFaaS Cloud](https://docs.openfaas.com/openfaas-cloud/intro/) introduces an automated build and management system for your Serverless functions with native integrations into your source-control management system whether that is GitHub or GitLab. As soon as OpenFaaS Cloud receives a push event from git it will run through a build-workflow which clones your repo, builds a Docker image, pushes it to a registry and then deploys your functions to your cluster. Each user can access and monitor their functions through their personal dashboard.

Can we scale manually using the REST API of the faas gateway?


**References:**
