---
layout: post
title: "Notes on Kubernetes"
categories:
  - notes
tags:
  - notes
---
# Notes
- Pod is the basic unit of execution in Kubernetes. It encapsulate application container (one or more), storage resources, unique network IP and container options.
- Containers within a Pod share namespace as well as IP address and port space.
- Containers within the Pod see the system's hostname as being same as the configured name for the Pod.
- One container per pod is the most common Kubernetes use case
- Controller creates, manages, handles replication and rollout, and provides self healing capabilities for pods.
- At the moment, Kubernetes does not support live updates of individual containers
- In GKE node pool is a group of nodes with an Kubernetes cluster that have the same configuration. The nodes in a node pool are identical to one another
- In GKE you can use [node taints](https://cloud.google.com/kubernetes-engine/docs/how-to/node-taints) to control where the Pods are scheduled
- Controllers and their types?
- You can define [preStop hook](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#hook-details) for the Pod's containers that'd be invoked if the Pod has been marked for termination
- Namespaces essentially allow users to have multiple virtual kubernetes cluster on the same physical cluster. It provides isolation by providing independent environments
- Three default namespaces: default, kube-system and kube-public
- Namespaces divide resources between users using resource quotes (e.g. number of nodes). Here's a [guide](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/) for configuring default memory requests and limits for a namespace
- Kubernetes has imperative object management and declarative management. Here are more details about the [Kubernetes Object Management](https://kubernetes.io/docs/concepts/overview/working-with-objects/object-management/)
- [Intro to resource requests and limits](https://cloud.google.com/blog/products/gcp/kubernetes-best-practices-resource-requests-and-limits)
- Types of services: ClusterIP, NodePort and LoadBalancer (Cloud specific)
- Master node runs: kube-apiserver, kube-scheduler (one across all master nodes), kube-controller-manager (one across all master nodes), kubelet, kube-proxy
- Node runs: kubelet
- Kube deployments mainly consist of: ingress, services, deployments


**References:**
- [Short Intro to Kubernetes Namespaces](https://opensource.com/article/19/12/kubernetes-namespaces)
