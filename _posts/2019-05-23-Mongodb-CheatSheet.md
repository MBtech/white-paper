---
layout: post
title: "MongoDB Cheatsheet"
categories:
  - guide
tags:
  - guide
---
This is just an combination of the MongoDB related commands that I tend to use often

# MongoDB CLI commands
The following commands are supposed to be executed from the MongoDB cli:

Show the available dbs:
`show dbs`

Shift the context to a particular db:
`use <db name>``

Show available collections in a db:
`show collections`

Print all the entries in a collection:
`db.<collection name>.find()`
or
`db['<collection name>'].find()`
or
`db['<collection name>'].find().pretty()`

Copy a collection:
`db['<collection name>'].copyTop('<target collection name>')`

Delete a collection:
`db['<collection name>'].drop()`

# CLI commands
Export a mongodb collection to JSON:
`mongoexport --collection <collection name> --out <collection name>.json`

Import a json file to mongodb collection:
`mongoimport --db <db name> --collection <collection name> --file <collection name>.json`



**References:**

https://stackoverflow.com/questions/13916004/mongo-copy-from-one-collection-to-another-on-the-same-db
