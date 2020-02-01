---
layout: post
layout: post
title: "Tutorial: Profiling Java Applications"
categories:
  - tutorial
  - csblog
tags:
  - content
---

## Profiling Java Applications

There are several tools to profile Java applications. There are command line as well as GUI tools to perform different profiling tasks for Java applications.

Command line tools include:

* hprof
* jstat
* jstack
* jhat

GUI tools:

* HPJMeter
* JVisualVM/VisualVM
* VisualGC
* YourKit Java Profiler
* jmc (Java mission control)
* jconsole
* JProfiler


### hprof:
hprof for heap and cpu profiling. hprof is actually a JVM native agent library which is dynamically loaded through a command line option, at JVM startup, and becomes part of the JVM process. By supplying HPROF options at startup, users can request various types of heap and/or cpu profiling features from HPROF. The data generated can be in textual or binary format, and can be used to track down and isolate performance problems involving memory usage and inefficient code.

The options to hprof are provided as a commas separated list. In order to get the list of options available for hprof use:

`java -agentlib:hprof=help`

Some of the types of profiling you can do with hprof are:

Heap Allocation profiles (heap=sites) which provides a sorted list of allocation sites. This identifies the most heavily allocated object types, and the TRACE at which those allocations occurred.

`java -agentlib:hprof=heap=sites -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`

Get a full heap dump

`java -agentlib:hprof=heap=dump,format=b -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`

If getting the output file in binary format you should use jhat tool to read that binary file

`jhat <filename>`

This creates an http server at port 7000 with the info in a presentable format. In addition you can execute queries on the heap dump using OQL (Object Query Language).

Get CPU samples

`java -agentlib:hprof=cpu=samples -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`

Get method invocations

`java -agentlib:hprof=cpu=times -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`

much more CPU intensive and uses Byte Code Injection into every method's entry and exit to track invocations

### jstat:
jstat is a monitoring tool for HotSpot JVM. jstat mainly provides information about GC, class loader operation and JIT compiler operation information.
`jstat -gc <pid> 10000`

`jstat -gcutil <pid> 10000`
to get percentage of space used for each region
Note: Forcing GC (if JDK>=7) jcmd 32377 GC.run

### -verbosegc option

`java -verbosegc -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`

use -Xloggc: filename to log the gc data into a file and you can later use this file in HPJmeter to visually analyze gc information.

Use -XX:+PrintGCDetails to get detailed log of the GC activities

In order to understand what output of different fields in verbosegc output means, take a look at this link http://javaeesupportpatterns.blogspot.be/2011/10/verbosegc-output-tutorial-java-7.html

### JvisualVM / VisualVM and VisualGC
Provides information similar to Yourkit Java profiler, however lacks several advanced information like thread blocking and deadlocks that can be easily seen in YourKit Java Profiler.

### YourKit Profiler

If you can not attach to an already running java application and you are asked to start the java application with the user agent you need to use the following way to start an application with a profiler agent:

`java -agentpath:/Users/mb/Downloads/YourKit-Java-Profiler-2016.02.app/Contents/Resources/bin/mac/libyjpagent.jnilib -verbosegc -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.PrimeCalculator`


Check for lock contention

`java -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.RunnableHello`

Check for deadlock

`java -cp target/ProfilerProject-1.0-SNAPSHOT.jar com.learningcurve.mb.profilerproject.Deadlock`

Remote profiling using YourKit https://www.yourkit.com/docs/java/help/remote_profiling.jsp

### Misc
Short introduction for Java Garbage Collectors:
http://blog.takipi.com/garbage-collectors-serial-vs-parallel-vs-cms-vs-the-g1-and-whats-new-in-java-8/

http://www.cubrid.org/blog/dev-platform/understanding-java-garbage-collection/

How to monitor GC activity:
http://www.cubrid.org/blog/dev-platform/how-to-monitor-java-garbage-collection/

How does hprof work? http://docs.oracle.com/javase/8/docs/technotes/samples/hprof.html

Unsecure JMX remote monitoring JVM options
`java -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.port=5000 -Dcom.sun.management.jmxremote.ssl=false -jar GameOfLife.jar`

Monitor throughput Jconsole
`jconsole hostname:port`


### Custom monitors
Java Attach API
https://blogs.oracle.com/CoreJavaTechTips/entry/the_attach_api
https://docs.oracle.com/javase/8/docs/technotes/guides/management/agent.html#gdenl


## References:
http://docs.oracle.com/javase/8/docs/technotes/samples/hprof.html

http://docs.oracle.com/javase/7/docs/technotes/tools/share/jhat.html

http://www.cubrid.org/blog/dev-platform/how-to-monitor-java-garbage-collection/

http://javaeesupportpatterns.blogspot.be/2011/10/verbosegc-output-tutorial-java-7.html

http://stackoverflow.com/questions/1385843/simple-deadlock-examples
