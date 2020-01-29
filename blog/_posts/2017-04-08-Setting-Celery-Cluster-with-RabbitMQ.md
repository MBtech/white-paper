---
layout: post
title: "Setting Up Celery Cluster with RabbitMQ"
categories:
  - tutorial
tags:
  - tutorial
---

## Setting up Celery Cluster with RabbitMQ

### Setup
I will be using 3 machines for this example. Let's say we have 1 Master node and 2 Worker nodes
We need to do the following steps on all of the nodes:

{% highlight bash %}
sudo pip install celery
sudo rabbitmqctl add_user <username> <password>
sudo rabbitmqctl add_vhost <vhost_name>
sudo rabbitmqctl set_permissions -p <vhost_name> <username> ".*" ".*" ".*"
{% endhighlight %}

You can replace <username>, <password> and <vhost_name> with what you want. We will use these names later on.

You can check the status of rabbit server using
`sudo rabbitmqctl status`
Start and stop your server using
`sudo rabbitmq-server -detached`
`sudo rabbitmqctl stop`

### Configuring and starting the cluster
Here is the summary of steps that you need to do for configuring and starting a rabbitmq cluster for your celery project
We need to configure the erlang cookie. Just make sure you have the same string in the .erlang.cookie file in all the servers.
Make sure you have RabbitMQ stopped on all nodes before changing the .erlang.cookie file.

{% highlight bash %}
sudo chmod 777 /var/lib/rabbitmq/.erlang.cookie
sudo vi /var/lib/rabbitmq/.erlang.cookie
sudo chmod 400 /var/lib/rabbitmq/.erlang.cookie
{% endhighlight %}

Make sure the `/etc/hosts` file on all machines have information about the other machines in the cluster. You can check that by doing a ping between machines

Start the RabbitMQ in detached mode on the server

Start the RabbitMQ server in detached mode on the worker nodes and perform the following steps

{% highlight bash %}
sudo rabbitmqctl stop_app
sudo rabbitmqctl reset
sudo rabbitmqctl cluster rabbit@<server_name/ip>
sudo rabbitmqctl start_app
{% endhighlight %}

You can check the nodes in the cluster using
`sudo rabbitmqctl cluster_status`

Starting a Celery Worker

`celery worker -A tasks -l info`

Celery worker with feeding queue specified
`celery worker -Q <queue_name>`

Use `celery multi start` for starting multiple workers


**References**
- https://www.virtualbox.org/manual/ch07.html
- https://timbull.com/build-a-processing-queue-with-multi-threading-and-spread-over-multiple-servers-in-less-than-a-day-26b0d7f5613b
- http://avilpage.com/2014/11/scaling-celery-sending-tasks-to-remote.html
