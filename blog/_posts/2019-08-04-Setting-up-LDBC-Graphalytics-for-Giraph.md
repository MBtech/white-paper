---
layout: post
title: "Getting Started with LDBC Graphalytics Benchmark for Giraph"
categories:
  - guide
tags:
  - guide
---
## Install Pre-requisites
Install Java 7+, Git, Maven, YARN, Zookeeper
You can follow [this guide](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-an-apache-zookeeper-cluster-on-ubuntu-18-04) to setup a zookeeper either in a single node more or a cluster mode. I will try to write up an ansible playbook for this task, soon (hopefully ... maybe ... perhaps). 
## Setup
Download core repo
git clone https://github.com/ldbc/ldbc_graphalytics.git

Install Graphalytics core to a local maven repo
mvn clean install

Clone platform driver repo
git clone https://github.com/atlarge-research/graphalytics-platforms-graphx.git

Build
mvn package

Download datasets from here
https://graphalytics.org/datasets

Now comes the most arduous part of the setup journey. Unfortunately, Graphalytics documentation simply leaves it completely up to you to configure YARN Cluster, which means that if you haven't done it before it could be a pain.
Here I will provide a sample configuration to ease your passing through the land of fire and blood.
Here's a link to the [ansible repo](https://github.com/MBtech/ansible-spark.git) on my github that will automate the process but I will explain some of the stuff that I had to look around for in order to get everything working.

Following the regular YARN setup guides will not be enough. Here are the extra things that I had to do:
```xml
<property>
<name>yarn.nodemanager.vmem-check-enabled</name>
<value>false</value>
</property>

<property>
<name>yarn.nodemanager.resource.memory-mb</name>
<value> {{ nodemanager_memory }} </value>
</property>

<property>
<name>yarn.scheduler.maximum-allocation-mb</name>
<value>{{ scheduler_max_allocation }}</value>
</property>
```
Without these configurations your container will likely run out of memory (physical or maximum allowable virtual memory). Even with small test data the defaults values were insufficient for a GraphX YARN Application.
When the virtual memory limits are exceeded without the `yarn.nodemanager.vmem-check-enabled` set to false, YARN gives off misleading error. Simply stating a `SIGTERM` in the logs or a connection failure to a container causing an exception. However, that is preceeded by the real reason in the form of a `WARN` message stating something like `Container X is running beyond virtual memory limits ..... Killing container.` This first property disables this default termination behavior. The other two properties are to increase the available memory for the YARN containers. Take a look at [this link](https://www.ibm.com/support/knowledgecenter/en/SSZJPZ_11.7.0/com.ibm.swg.im.iis.ishadoop.doc/topics/configuring_hadoop.html) for details about all these properties.


[Here's is a decent usual guide to setup a YARN Cluster](https://www.linode.com/docs/databases/hadoop/install-configure-run-spark-on-top-of-hadoop-yarn-cluster/)
