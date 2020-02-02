---
layout: post
title: "Notes on Openwhisk codebase"
categories:
  - guide
tags:
  - guide
---
## Notes

CPU shares in docker can be adjusted using `-c`, `--cpu-shares` the full share is 1024. [Related SO Answer](https://stackoverflow.com/questions/26841846/how-to-allocate-50-cpu-resource-to-docker-container-via-docker-run). This is a relative weightage and might not be the best way for strict control. The actual amount of CPU time would depend on the other containers running on the system.
`--cpus` flag may be more appropriate for strict control. How does this flag behave in multi-core setting

Memory limit is set using `-m` or `--memory`

[Docker run constraints](https://docs.docker.com/engine/reference/run/#/runtime-constraints-on-resources)

[DockerContainer](https://github.com/apache/openwhisk/blob/231e739373ef681c44b5647a6956d5838a87db2e/core/invoker/src/main/scala/org/apache/openwhisk/core/containerpool/docker/DockerContainer.scala) contain the object to start create, start and run docker containers.

Openwhisk seems to have cpu share argument explicity but not explicity cpu argument. It does have container args config which perhaps can be used to specify custom arguments
These arguments are passed through a configuration called `whisk.container-factory.container-args` 
Not sure how it actually works

**References:**
