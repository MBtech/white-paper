---
layout: post
title: "Redis Cheatsheet"
categories:
  - guide
tags:
  - guide
---
This is just an combination of the Redis related commands that I tend to use often

List databases for which keys are defined

`> INFO keyspace` or `redis-cli INFO keyspace`

Delete all keys from all redis databases:
`$ redis-cli FLUSHALL`

Delete all keys of a specific database:
`$ redis-cli -n <database_number> FLUSHDB`

### Disabling persistence in Redis
To disable all data persistence in Redis change the following in `redis.conf`:

- Disable AOF by setting the appendonly configuration directive to no (it is the default value)
- Disable RDB snapshotting by disabling (commenting out) all of the saveconfiguration directives (there are 3 that are defined by default)


**References:**
