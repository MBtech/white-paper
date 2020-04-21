---
layout: post
title: "Notes on Kafka"
categories:
  - notes
tags:
  - notes
---
## Kafka Basic Concepts
- Topics: A particular stream of data. You can have as many topics as you want and they are identified by name
- Partitions: Topics are split in partitions. Each partition is ordered. Each message within a partition gets an incremental id. This ID is called Offset
- You have to specify the number of partition when creating a topic. This number of partition can be changed later 
- Offset have only meaning for a specific partition. 
- Data is kept only for a limited period. However the offset keep on incrementing even when the data is deleted 
- Immutability 
- Data is by default assigned randomly to a partition but a key can be used to route data to specific partitions
- Brokers: A kafka cluster is composed of multiple brokers and brokers hold the partition. Each broker has an ID
- Replication: Topics are generally replicated. Each partition of a topic is replicated
- At any time only one broker can be a leader for a given partition. Only that leader can receive or serve data for a partition 
- Zookeeper is used for leader election
- Producers: Write data to topic. They automatically know to which broker and partition to write data to. Producer can choose to receive acknowledgements of data writes. ack=0 producer won't wait for ack. ack=1 is the default with leader ack required. ack=all acks for all brokers required
- Producer can choose to send a key with a message. That key ensures that message for that key will go to the same partition 
- Consumers read data from a topic. Data is read in order within each partition 
- Consumer groups: Parallel instances of consumers grouped together. Each consumer in a group reads from exclusive partitions. If you have more consumers in a group than the number of partitions in the topic they are reading from, some consumers will be inactive 
- Consumer Offsets: Kafka stores the offsets at which a consumer group has been reading. Once a consumer processes the data it should commit the offset
- The offsets are commited live in a Kafka topic named `__consumer_offsets`
- The decision of when to commit offset leads to different deliery semantics: Right after message is received (at most once), Comit after message is processed (atleast once), Exactly once is only available with kafka workflows using Kafka streams API
- Zookeeper managers brokers, performs leader election and sends notification to Kafka for changes (e.g. new topic, broker dies, broker comes up, deleting topics). Zookeeper does not store offsets with Kafka > 0.10