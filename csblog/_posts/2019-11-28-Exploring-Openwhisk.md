---
layout: post
title: "Exploring Openwhisk"
categories:
  - guide
tags:
  - guide
---
# Openwhisk

Controller is written in scala and uses akka actors
====================

2016 talk on under the hood
mention the use of kafka sdk, couchdb sdk, spray dsl and consul sdk (verify)
Load balancer to decide where to run the action
Kafka is used to queue action requests to be executed
Openwhisk uses something called a stem cell container that is essentially a container up and running but not specialized for particular user actions

====================

OpenWhisk has something called Service Provider Interface (SPI) extensions that lets users implement their own implementation for services e.g. their own scheduler.

====================

![OpenWhisk Architecture]('/images/OpenWhisk_flow_of_processing.png')

Nginx: The HTTP and reverse proxy server for OpenWhisk. Openwhisk user-facing API is completely HTTP based and follows RESTful design

Controller: Controller parses and progress the HTTP requests, communicates with CouchDB for authentication and authorization information, contains the load balancer that makes the decision regarding which invoker to submit the function to Kafka persist queue

CouchDB: Is used to store information about available functions (actions), authentication and authorization info and results.

Kafka: Serves as the message queue for job submission to different invokers

Invoker: Invoker is the heart of OpenWhisk and invokes actions using docker containers. [This article from 2017](https://thenewstack.io/behind-scenes-apache-openwhisk-serverless-platform/) but it suggests that "The Invoker makes the decision of either reusing an existing 'hot' container, or starting a paused 'warm' container, or launching a new 'cold' container for a new invocation." Not sure if this is still true.

==========

OpenWhisk support blocking (synchronous), non-blocking (asynchronous) and periodic invocation models.

It does not seem like OpenWhisk allows any control over CPU resources by default

Local deployment is recommended with Docker compose while production deployment is recommended with Kubernetes


While Kubernetes (and other container orchestration systems) are designed to manage containers, they cannot yet handle the scale at which containers are created and destroyed in a functions-as-a-service model. The container pool manager in OpenWhisk manages speculative provisioning of resources, and is designed to enhance container locality (i.e., reuse a resource for a given function) to reduce system overhead.
The invoker also relies on container suspend and resume operations through lower level protocols that bypass the Kubernetes controller. These also shorten the critical path when invoking a function. In a production setting, OpenWhisk will spawn, resuse, and destroy millions of containers a day. The bespoke resource scheduling allows for a system overhead of 10 ms or less on average.

CouchDB database "whisks" contains the information and code about the Action which should also contain the information about resource restrictions (is CPU restriction available?)
==========

What is NodeRED? What is Kong?

## Getting Started
Make sure your default shell is bash. Install npm and zip 
```bash
brew install wsk
```
### Quick start with Docker-compose
Run the following commands
```bash
git clone https://github.com/apache/openwhisk-devtools.git
cd openwhisk-devtools/docker-compose
make quick-start
```
This will do the following among other things:
Download the wsk cli tool, download the openwhisk/controller container, openwhisk/nodejs6action container, openwhisk/dockerskeleton, openwhisk/invoker, openwhisk/apigateway container,
**References:**
- [How OpenWhisk works](https://github.com/apache/openwhisk/blob/master/docs/about.md#how-openWhisk-works)
