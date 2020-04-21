---
layout: post
title: "CheatSheet for Kubernetes"
categories:
  - cheatsheet
  - csblog
tags:
  - cheatsheet
---

Get pods and their status: `kubectl get pods`. Use `-n` flag if you want to get pods from a particular namespace

Detailed information about the pods: `kubectl describe pod`

Delete a pod: `kubectl delete <podname>`

Get namespaces: `kubectl get namespace`

Create namespace: `kubectl create namespace <namespace>`

Delete namespace: `kubectl delete ns <namespace>`

Get logs: `kubectl logs -n <namespace> -c <containername> <podname>`
`-f` for streaming and `-p` for previously terminated containers

Triggering rollout changes: `kubectl rollout restart deployment <appname>`. You can also change `imagePullPolicy: Always` in the deployment manifest to always pull the container image

Set image for a deployment: `kubectl set image deployment/gateway gateway=mbilalce/gateway:latest-dev -n openfaas`

Edit the deployment config: `kubectl edit deployment gateway -n openfaas`

Get deployments: `kubectl get deployments -n openfaas`

Get yaml for deployment: `kubectl get deploy <deploymentname> -o yaml --export`

Get a shell to a running container: `kubectl exec -it <pod_name> -- /bin/bash`

If the pod contains more than one container then use: `kubectl exec -it <pod_name> --container <container_name> -- /bin/bash`
**References:**
- [Terminated Container logs](https://stackoverflow.com/questions/57007134/kubernetes-how-to-see-logs-of-terminated-pods)
- https://stackoverflow.com/questions/33112789/how-do-i-force-kubernetes-to-re-pull-an-image
- [Change image for a container](https://stackoverflow.com/questions/40366192/kubernetes-how-to-make-deployment-to-update-image)
