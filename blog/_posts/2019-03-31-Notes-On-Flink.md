---
layout: post
title: "Notes on Flink"
categories:
  - guide
tags:
  - guide
---

Updated 31-03-2019
Here are a few notes on Apache Flink that I have gathered while I was looking into some internals:

## JobManager and TaskManager
The JobManagers (also called masters) coordinate the distributed execution. They schedule tasks, coordinate checkpoints, coordinate recovery on failures, etc.

There is always at least one Job Manager. A high-availability setup will have multiple JobManagers, one of which one is always the leader, and the others are standby.

The TaskManagers (also called workers) execute the tasks (or more specifically, the subtasks) of a dataflow, and buffer and exchange the data streams.

Each worker (TaskManager) is a JVM process, and may execute one or more subtasks in separate threads. To control how many tasks a worker accepts, a worker has so called task slots (at least one).

Information about the Distributed Runtime Environment of Flink can be found [here](https://ci.apache.org/projects/flink/flink-docs-master/concepts/runtime.html#task-slots-and-resources).

## Slot, SlotSharingGroup and CoLocationGroup
A slot defines a fixed slice of resources of a TaskManager. Every subtask (parallel instance of an operator) needs a slot in order to be executed.

Since not all operators are equally resource intensive, some of them need more memory or cpu cycles than others. In order to better utilize resources, Flink allows subtasks of different operators to be deployed into the same slot.

Which operators can be deployed into the same slot is controlled by the SlotSharingGroup. Tasks which share the same slot sharing group can be executed in the same slot and, thus, share resources. By default, all operators are assigned the same SlotSharingGroup.

SlotSharingGroup and CoLocationGroup different in terms of strictness. SlotSharingGroup is a soft permission while CoLocationGroup is a hard constraint.

More information about job scheduling can be found [here](https://ci.apache.org/projects/flink/flink-docs-master/internals/job_scheduling.html).

**References:**
1. https://stackoverflow.com/questions/50741695/what-is-slotsharinggroup-in-apache-flink
