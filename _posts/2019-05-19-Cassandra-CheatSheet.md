---
layout: post
title: "Cassandra Cheatsheet"
categories:
  - guide
tags:
  - guide
---
This is just an combination of the Cassandra related commands that I tend to use often

# Cassandra CLI commands
The following commands are supposed to be executed from the cassandra cli:

Access cassandra CLI
`cqlsh [<hostname>]`

Dropping a table
`DROP TABLE <table name>;`

Get a replication strategy for a keyspace
`SELECT * FROM system_schema.keyspaces;`

Alter the replication strategy or replication factor of a keyspace
`ALTER KEYSPACE "<keyspace name>" WITH REPLICATION =
  {'class' : 'SimpleStrategy', 'replication_factor': 3};`

You would need to run `nodetool repair -full` for each node for an existing keyspace


# Cassandra CLI Tools
Statistics about a table on a node

`nodetool cfstats -H -- <keyspace>.<table>`

-H provides a more human readable form

**References:**
