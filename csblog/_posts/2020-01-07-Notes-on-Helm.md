---
layout: post
title: "Notes on Helm"
categories:
  - notes
  - csblog
tags:
  - notes
---
## Notes

A helm package that contains information sufficient for installing a set of Kubernetes resources into a Kubernetes cluster. Charts contain a `Chart.yaml` file as well as templates, default values `values.yaml` and dependency

A chart archived is a tarred and gzipped chart

Helm is the package manager for Kubernetes

Kube config: Helm client learns about Kubernetes clusters by using kube config. By default Helm attemps to find this in $HOME/.kube/config

Helm charts can have provenance file that provides information about where the chart came from and what it contains.

Installing a helm chart creates a release to track that installation

`values.yaml` can be used to override template defaults

Helm chart repositories are HTTP server that serves `index.yaml`

**References:**
