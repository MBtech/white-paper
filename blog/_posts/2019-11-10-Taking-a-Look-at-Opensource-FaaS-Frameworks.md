---
layout: post
title: "Opensource FaaS Platforms"
categories:
  - guide
tags:
  - guide
---
# Openwhisk
Written in scala
Supports Kubernetes 
Controller is written in scala and uses akka actors
====================
2016 talk on under the hood
mention the use of kafka sdk, couchdb sdk, spray dsl and consul sdk (verify)
Load balancer to decide where to run the action
Kafka is used to queue action requests to be executed
Openwhisk uses something called a stem cell container that is essentially a container up and running but not specialized for particular user actions
====================

OpenWhisk has something called Service Provider Interface (SPI) extensions that lets users implement their own implementation for services e.g. their own scheduler.

========
There are proposals for [Autonomous Container scheduler](https://cwiki.apache.org/confluence/display/OPENWHISK/Autonomous+Container+Scheduler+v2) for Openwhisk. Where they accepted?


# OpenLambda
Built in go-lang
A research platform based on [HotCloud'16 paper](https://www.usenix.org/system/files/conference/hotcloud16/hotcloud16_hendrickson.pdf)


# Fission
Written in go-lang

# Funktion

# Kubeless
Written in go-lang
Kubernetes Native

# OpenFaaS
Written in go-lang
Supports Kubernetes and Docker Swarm

# IronFunctions

# Fn Project
Also written in go-lang

# TriggerMesh
Cross-cloud triggers

# AppScale
Written mainly in Python
Opensource and modeled on Google App Engine APIs to allow develoeprs to automatically deploy and
scale unmodified Google App Engineer applications over public, private and on-premise cloud.

# Dispatch
Written in Golang
Supported by VMware
# Galactic Fog's Gestalt
Seems not updated and possibly decommissioned

# Nuclio
Written mainly in golang

# Riff

# Knative from google

# LunchBadger

# Osiris

# Keda from Azure

**References:**
- https://vshn.ch/en/blog/a-very-quick-comparison-of-kubernetes-serverless-frameworks/
