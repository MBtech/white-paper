---
layout: post
title: "MongoDB Cheatsheet"
categories:
  - guide
tags:
  - guide
---
This is just an combination of the MongoDB related commands that I tend to use often
Updated: 7/8/2019

# MongoDB CLI commands
The following commands are supposed to be executed from the MongoDB cli:

Show the available dbs:
`show dbs`

Shift the context to a particular db or create a database:
`use <db name>`

Show available collections in a db:
`show collections`

Drop a database:
`db.dropDatabase()`

Create a collection (options are fittingly optional):
`db.createCollection(<name>, <options>)`

Print all the entries in a collection:
`db.<collection name>.find()`
or
`db['<collection name>'].find()`
or
`db['<collection name>'].find().pretty()`

Return only specific fields
`db['<collection name>'].find( { }, { <field name 1>: 1, <field name 2>: 1 } )`

Return matching entries
`db['<collection name>'].find({ "id": 5})`

Query with ranges
`db['<collection name>'].find( { birth: { $gt: new Date('1940-01-01'), $lt: new Date('1960-01-01') } } )`

Copy a collection:
`db['<collection name>'].copyTop('<target collection name>')`

Delete a collection:
`db['<collection name>'].drop()`

Insert a document:
`db['<collection name>'].insert(DOCUMENT)`

Update a document:
```
db.inventory.updateOne(
   { item: <matching criteria for item> },
   {
     $set: { "size": "cm" }
   }
)
```

Update multiple documents:
```
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size": "in" }
     $currentDate: { lastModified: true }
   }
)
```
Here we also create a new field `currentDate` for the documents on which our criteria matches

Delete a document, given a criteria:
`db['<collection name>'].remove(CRITERIA)`

For example if you are matching on the title key in the document:
`db['<collection name>'].remove({'title' : 'MongoDB'}}`

# Linux CLI commands
Export a mongodb collection to JSON:
`mongoexport --collection <collection name> --out <collection name>.json`

Import a json file to mongodb collection:
`mongoimport --db <db name> --collection <collection name> --file <collection name>.json`


**References:**
- https://stackoverflow.com/questions/13916004/mongo-copy-from-one-collection-to-another-on-the-same-db
- https://docs.mongodb.com/manual/reference/method/db.collection.find/
- https://docs.mongodb.com/manual/tutorial/update-documents/
