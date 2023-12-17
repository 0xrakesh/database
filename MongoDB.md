# Mongodb
&emsp;MongoDB is a popular, open-source NoSQL (non-relational) database management system. It was developed by MongoDB Inc. and is designed to handle large volumes of data with a flexible, schema-less data model. MongoDB stores data in a binary JSON-like format called **BSON (Binary JSON)**, which allows for rich data structures, making it well-suited for a variety of applications.

## When to use MongoDB?
&emsp;When dealing with **large amounts of data** that may require extensive read and write operations, MongoDB is an excellent choice due to its high performance and horizontal scaling. By leveraging replication and sharding, you can distribute data across multiple servers, reducing the workload on a single machine.

## Features
1. Document-Oriented
2. Collections
3. Schema-less
4. Indexing
5. Query Language
6. Replication
7. Flexible Schema.
8. High Availability
9. Real-Time Analytics.
10. Geo-spatial Queries.
11. Rapid Application Development.

## Atlas
1. Database as a Service.
2. Global Cluster Support.
3. Security.
4. Performance.
5. Easy Scaling.
6. Data Automation and Integration.

## MongoDB Terminology
1. Database.
2. Collection.
3. Document.
4. Field.
5. Index.
6. Query.
7. Cursor.
8. Aggregation.
9. Replica Set.
10. Sharding.

## About
&emsp;It is a document-oriented database, not a relational one. Need of mongodb is scaling out easier. The concept **row** is replaced by **document** and **table**. There is no predefined schemas. A document's key and values are not of fixed types or size. 

&emsp;Mongodb was designed to scale out. It is easier to split data  across multiple servers. 

&emsp;A document is the basic unit of data for MongoDB. Every document has a special key, \_id, that is unique within a key. 

## Documents
&emsp;At heart of MongoDB is the document. an ordered set of keys with associated values.
```
document: {"key":"value"}
```

## Collections
&emsp;A collection is a group of documents.
**Dynamics schemas**: Collections have dynamic schemas, that documents within a single collection can have any number of different "shapes".

## Database.
&emsp;A Databse is a group of collection of documents.
**Default database**: admin - authentication and authorization. config - shard database, local - replication database.


# Basic Commands
**CRUD**
```~js
// CREATE
db.collection.insertOne({"key":"value"})

// READ
db.collection.find().pretty();

// UPDATE
db.collection.updateOne({findKey},{updateOpration})

// DELETE
db.collection.deleteOne({findKey})
db.collection.deleteMany({findKey})
```

# Data Types
1. Null
2. Boolean
3. Number
4. String
5. Date
6. Array
7. Embedded document ( Nested document ).
8. Object ID
9. Binary data.
10. Code

# Insert
```
db.collection.insertOne({key:value})

db.collection.insertMany([key1:value1, key2:value2])
```

# Delete
```
db.collection.deleteOne({key:value})

db.collection.deleteMany({key:value})

db.collection.drop()
```

# Update
```
db.collection.updateOne({find},{update})

db.collection.updateMany({find},{update})
```
**Operations**
1. set
2. inc
3. dec
4. push
5. addToSet
6. pop
7. pull
8. sort - (*Third argument*)
9. gte - (_Find operation_)
10. lte - (_Find operation_)
11. lt - (_Find operation_)
12. gt - (*Find operation*)
13. in - (_Find operation_)
14. not - (_Find operaion_)
15. where - (_Find operation_)

**Function**
1. limit
2. sort
3. skip


# Index
&emsp;A database index is a similiar to a book's index. Instead of looking through the whole collection, just the looking the index of the finding value in database index.

```
db.collection.createIndex({key:value})

// Check the execution Statatics
db.collection.find({key:value}).explain("executionStats")
```

# Compund Indexes
&emsp;Building indexes based on two or more keys. It is useful if your query has multiple sort directions or multiple keys in the criteria. A compound index is an index on more than one field
`db.collection.createIndex({'key':'value','key':'value'})`

# Bulk Write
&emsp;There are two type of bulk writing.
- Ordered Bulk Write - If a write operation fails, MongoDB return an error and does not proceed with the remaining operations
- Unordered Bulk Write - If a write operation fails, MongoDB will continue to process the remaining write operations.

**Ordered Bulk**
```js
const orderBulk = db.collection('name').initializeOrderedBulkOp();

orderedBulk.insert({_id:1, name:"John Doe"});
orderedBulk.find({_id:2}).updateOne({$set: {name:'Jane Doe'}});
orderedBulk.find({_id:3}).remove();

orderedBulk.execute((err,res) => {
	// Statement
})
```

**Unordered Bulk**
```js
const orderBulk = db.collection('name').initializeUnorderedBulkOp();

orderedBulk.insert({_id:1, name:"John Doe"});
orderedBulk.find({_id:2}).updateOne({$set: {name:'Jane Doe'}});
orderedBulk.find({_id:3}).remove();

orderedBulk.execute((err,res) => {
	// Statement
})
```

**Validate Function**
&emsp;The validate command is used to examine a MongoDB collection to verify and report on the correctness of its internal structures, such as indexes, namespace details, or documents. Return statistics about the storage and distribution of data within a collection.
```js
db.runCommand({validate: "collectionName", options...});
```
**Option**
- full
- background

# Counting documents
&emsp;Used to find the number of documents present in a collection. MongoDB provides some methods:
- countDocuments()
- estimatedDocumentCount()

**countDocuments**
```js
collection.countDocuments(filter, options);

db.collection("name").countDocuments({status: 'completed'}, (err, count) =>
	{
		console.log(count);
	});
```


**estimatedDocumentCount**
```js
db.collection('name').estimatedDocumentCount((err,count) => {
	console.log(count);
})
```

&ensp;
# Read / Write Concerns
&emsp;Read and write concerns are crucial aspects of data consistency and reliability in MongoDB. They determine the level of acknowledgement required by the database for read and write operations.

## Read Concern
&emsp; It determine the consistency level of the data returned by a query and specifies the version of data that a query should return.
- local - Returns the most recent data available on the primary node at the time of query execution.
- availabe - The query returns the most recent data available on the queried node.
- majority -  The query returns data that has been acknowledged by a majority of replica set members.
- linearizable - Ensures reading the most recent data that has been acknowledged by a majority of replica sets.
- snapshot - Returns the data from a specific snapshot timestamp. 

## Write Concern
&emsp;A write concern indicates the level of acknowledgment MongoDB should provide when writing data to the database. It ensures that the data has been successfully written and replicated before acknowledging the write operation.
- w:0 - unacknowledged, which means MongoDB does not send any acknowledgement. **lowest latency**
- w:1 - acknowledged that sucessfully written to primary node.
- w:majority - The write operation is acknowledged after being written and replicated to a majority of replica set members.

&ensp;
# Cursors
&emsp;A cursor is an object that enables you to iterate over and retrieve documents from a query result. When you execute a query to fetch documents from a database, MongoDB returns a pointer to the result set, known as a cursor. The cursor automatically takes care of batch processing of the result documents, providing an efficient way to handle large amounts of data.
```js
const cursor = db.collection('myCollection').find();
cursor.forEach((doc) => {
  console.log(doc);
});
```

**Cursor Methods**
- count()
- limit()
- skip()
- sort()
- project()

```js
const cursor = db.collection("name").find({age:{$gt:25}}).sort('name':1).limit(10).skip(20).project({name:1,_id:0});
```

**Close cursor**
```js
cursor.close();
```


# Query operations
- Projection Operators
- Comparison Operators
- Array Operators
- Logical Operators
- Element Operators

## Projection Operators
&emsp;The project is a projection operator in MongoDB, which is used during the aggregation process to reshape/output a document by specifying the fields to include or exclude.

**Include**
```js
{$project:{field:express}}

{
  "_id": 1,
  "name": "John Doe",
  "posts": [
    { "title": "Sample Post 1", "views": 43 },
    { "title": "Sample Post 2", "views": 89 }
  ]
}

db.users.aggregate([
  {
    $project: {
      name: 1,
      totalPosts: { $size: '$posts' },
    },
  },
]);

// Output
{
  "_id": 1,
  "name": "John Doe",
  "totalPosts": 2
}
```

**Exclude**
```js
db.users.aggregate([
  {
    $project: {
      posts: 0,
    },
  },
]);
```

## Include Operator
&emsp;The $include projection operator is used in queries to specify the fields that should be returned in the result documents. 
**Syntax**
`{field:1}`
```js
[
  {
    title: 'The Catcher in the Rye',
    author: 'J.D. Salinger',
    year: 1951,
    genre: 'Literary fiction',
  },
  {
    title: 'To Kill a Mockingbird',
    author: 'Harper Lee',
    year: 1960,
    genre: 'Southern Gothic',
  }
];

db.books.find({}, { title: 1, author: 1, _id: 0 });
```

&ensp;
## Exclude Operator
&emsp;The $include projection operator is used in queries to specify the fields that should be returned in the result documents. 
**Syntax**
`{field:0}`
```js
[
  {
    title: 'The Catcher in the Rye',
    author: 'J.D. Salinger',
    year: 1951,
    genre: 'Literary fiction',
  },
  {
    title: 'To Kill a Mockingbird',
    author: 'Harper Lee',
    year: 1960,
    genre: 'Southern Gothic',
  }
];

db.books.find({}, { title: 1, author: 0, _id: 0 });
```

&ensp;
## Slice Operator
&emsp;The $slice projection operator is a MongoDB feature that allows you to limit the number of elements returned for an array field within the documents.
```js
db.collection.find({}, { tags: { $slice: [4, 3] } });
```
&ensp;&ensp;
## Comparison Operator
&emsp;Comparison operators are used to performing various operations like comparing values or selecting documents based on the comparison. 
- $eq - `db.collection.find({ age: { $eq: 25 } });`
- $ne - `db.collection.find({ age: { $ne: 25 } });`
- $gt - `db.collection.find({ age: { $gt: 25 } });`
- $gle - `db.collection.find({ age: { $gte: 25 } });`
- $lt - `db.collection.find({ age: { $lt: 25 } });`
- $lte - `db.collection.find({ age: { $lte: 25 } });`

&ensp;
## Array Operators
&emsp;In MongoDB, array operators allow you to perform various operations on arrays within documents. These operators help you query and manipulate the elements in the array fields of your collections.

- $elemMatch - `db.collection.find({ scores: { $elemMatch: { $gte: 80, $lt: 90 } } });
`

- $all - `db.collection.find({ tags: { $all: ['mongodb', 'database'] } });`

- $size - `db.collection.find({ comments: { $size: 3 } });`
- $addToSet - `db.collection.updateOne({ _id: 1 }, { $addToSet: { colors: 'green' } });
`
- $push - `db.collection.updateOne({ _id: 1 }, { $push: { comments: 'Great article!' } });
`

&ensp;

# Indexes
&emsp;This smaller version is stored in an efficient manner, making it easier and faster to locate specific documents based on the indexed field(s).

## Types of indexes
- **Single Field**:Index based on a single field in the documents.
- **Compound Index**: Index based on multiple fields in the documents.
- **Multikey Index**: Index used when the indexed field contains an array of values.
- **Text Index**: Index used to support text search queries on string content.
- **2dsphere Index**: Index used to support geospatial queries on spherical data.
- **2d Index:** Index used to support geospatial queries on planar data.

```js
db.users.createIndex({ username: 1 });
db.users.createIndex({ username: 1, email: 1 });

db.users.find({ username: 'John' }).explain();
```

## Managing Indexes
- List all the indexes in a collection: db.COLLECTION_NAME.getIndexes().
- Remove an index: db.COLLECTION_NAME.dropIndex(INDEX_NAME).
- Remove all indexes: db.COLLECTION_NAME.dropIndexes().

## Unique Single Field Index
```js
db.users.createIndex({ email: 1 }, { unique: true });
```

&ensp;

# MongoDump
&emsp;Mongodump is a utility tool that comes with MongoDB, which is used to create a backup of your data by capturing the BSON output from your MongoDB database. It is especially useful when you want to export data from MongoDB instances, clusters or replica sets for either backup purposes or to migrate data from one environment to another.
```bash
mongodump --uri "mongodb://username:password@host:port/database" --out /path/to/output/dir
```

**Options**
- --uri: The MongoDB connection string with authentication details.
- --out: The path to save the output data.
- --db: The specific database to backup.
- --collection: The specific collection to backup.
- --query: An optional query to export only matching documents.
- --oplog: Include oplog data for a consistent point-in-time snapshot.
- --gzip: Compress the backup files using gzip.
- --archive: Write the output to a single archive file instead of individual files.
&ensp;
## Restoring data
```bash
mongorestore --uri "mongodb://username:password@host:port/database" --drop /path/to/backup/dir
```

&ensp;
# Role-based access control
&emsp;Role-based access control is an approach to restrict the access of users to perform certain tasks, view data or execute specific commands.

## Build-in Roles
1. Read
2. ReadWrite
3. dbAdmin
4. userAdmin
5. ClusterAdmin
6. ReadAnyDatabase
7. ReadWriteAnyDatabase
8. userAdminAnyDatabase
9. dbAdminAnyDatabase

## Custom Roles
&emsp;In addition to the built-in roles, you can create custom roles to cater to specific requirements of your application. Custom roles can have any combination of built-in roles’ privileges and user-defined actions.

```js
db.createRole({
  role: 'customRole',
  privileges: [
    {
      resource: { db: 'exampleDB', collection: '' },
      actions: ['find', 'insert', 'update', 'remove'],
    },
  ],
  roles: [],
});
```

## Create a user and assign role
&emsp;To ensure that users have the appropriate level of access and permissions, you assign specific roles to them. When creating a new user, you can assign roles using the db.createUser() method.

```js
db.createUser({
  user: 'exampleUser',
  pwd: 'examplePassword',
  roles: [
    { role: 'read', db: 'exampleDB' },
    { role: 'customRole', db: 'exampleDB' },
  ],
});
```


# X.509 Certificate Auth
&emsp;X.509 certificate authentication is a crucial aspect of MongoDB security that enables clients to verify each other’s authenticity using public key infrastructure (PKI). With X.509 certificate authentication, both the client and MongoDB server confirm the identity of the other party, ensuring secure communication and preventing unauthorized access.
