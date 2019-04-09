---
layout: post
title: "On Spark"
categories:
  - guide
tags:
  - guide
---
Here's my notes on random things about GraphX internals. Since I am not an expert in Spark or GraphX. Take them with a grain of salt. I am learning it myself. These notes are a result of looking into document as well as code and experimenting with the code itself.

### Property Graph:

The property graph implementation is composed of 2 RDD. A VertexRDD and a replicatedVertexView RDD. replicatedVertexView contains an EdgeRDD as well.

Implementing `getPreferredLocations()` function in the constructor of both EdgeRDD and VertexRDD doesn't allow you make spark take into account your suggestions. I am investigating the reason for that.

### On Partitioning:

I found out that if you graph is not partitioned into the number of partitions that you want when you first read the graph from an edge list file the number of tasks per stage can be rather odd. Take a look at this [question](https://stackoverflow.com/questions/55557607/number-of-tasks-per-stage-in-spark). I am trying to figure out the reason for this. 
The solution is to simply provide the desired number of partitions to the `GraphLoader.edgeListFile()` function.


**References:**

1. https://spark.apache.org/docs/latest/graphx-programming-guide.html
