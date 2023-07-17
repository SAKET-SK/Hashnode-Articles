---
title: "Day 5: MongoDB - Indexing and Performance Optimization"
datePublished: Mon Jul 17 2023 10:16:30 GMT+0000 (Coordinated Universal Time)
cuid: clk6pnsoj000h09l81zzn0lim
slug: day-5-mongodb-indexing-and-performance-optimization
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/YV2Nchv9pd4/upload/932733770b6d2ec3484bcea7129450dd.jpeg
tags: mongodb, backend, databases

---

Indexing is a powerful technique that significantly enhances query performance and overall database efficiency. In this article, we'll explore the art of indexing in MongoDB and delve into performance optimization strategies to make your database operations lightning-fast and seamless.

So, what is Indexing in MongoDB? Well, in MongoDB, indexing involves creating special data structures that allow the database to locate and retrieve documents quickly. Indexes are based on specific fields within a collection, facilitating faster data retrieval and reducing the need for full collection scans. By carefully designing indexes, you can dramatically improve the response time of your queries and enhance overall database performance.

Again we will rewind to our Collection which we were using previously.

```javascript
> db.students.find()
[
  {
    _id: ObjectId("64b0ed7a88c9597b454ae8fa"),
    id: 'TI001',
    Name: 'ABC',
    Credits: 7.8,
    City: 'Pune'
  },
  {
    _id: ObjectId("64b0eec888c9597b454ae900"),
    id: 'TI002',
    Name: 'XYZ',
    Credits: 7.3,
    City: 'Kolkata'
  },
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    id: 'TI004',
    Name: 'KLM',
    Credits: 8.2,
    City: 'Hyderabad'
  },
  {
    _id: ObjectId("64b222bc0578e3efed8dc01a"),
    id: 'TI003',
    Name: 'MOD',
    Credits: 6.6,
    City: 'Delhi',
    Languages: [ 'C', 'C++', 'Java', 'Python' ]
  },
  {
    _id: ObjectId("64b223100578e3efed8dc01b"),
    id: 'TI005',
    Name: 'ZKG',
    Credits: 6.6,
    City: 'Delhi',
    Languages: [ 'C', 'C++', 'Java' ]
  }
]
```

Suppose I want to create an Index on the "Name" and "ID" fields. So I will fire up this command:

```javascript
> db.students.createIndex({"Name":1})
Name_1
```

We have created the index on Student Name which means when someone searches the document based on the Name of the Student, the search will be faster because the index will be used for this search. So this is important to create the index on the field that will be frequently searched in a collection. The "Name\_1" is the name of the index which gets returned. To get the list of Indexes, we can use:

```javascript
> db.students.getIndexes()
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { Name: 1 }, name: 'Name_1' }
]
```

As you can see there are 2 indexes in our Collection. The value '1' denotes the ascending order while the '-1' denotes the descending order. Now let us try to drop the "Name\_1" index.

```javascript
> db.students.dropIndex({"Name":1})
{
  nIndexesWas: 2,
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1689587039, i: 6 }),
    signature: {
      hash: Binary(Buffer.from("17eaf70dd0b84405a67d3bd06ad327ce1ce79dbf", "hex"), 0),
      keyId: Long("7200818374631227437")
    }
  },
  operationTime: Timestamp({ t: 1689587039, i: 6 })
}
```

Now let us check if our index was dropped or not. We will use the getIndexes() method again to check.

```javascript
> db.students.getIndexes()
[ { v: 2, key: { _id: 1 }, name: '_id_' } ]
```

As you can see, the Name\_1 index has been dropped. You can also directly specify the name of the index in the "dropIndex()" method, like the below example:

```javascript
> db.students.dropIndex("Name_1")
{
  nIndexesWas: 2,
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1689587595, i: 12 }),
    signature: {
      hash: Binary(Buffer.from("d957372357eb59d2ada0e690b4009f43dd85e1dd", "hex"), 0),
      keyId: Long("7200818374631227437")
    }
  },
  operationTime: Timestamp({ t: 1689587595, i: 11 })
}
```

You can also specify more than one field while indexing. Suppose, I want to provide an Index in ascending order to "Name" and descending order to "Credits", then I can use this command:

```javascript
> db.students.createIndex({"Name":1,"Credits":-1})
Name_1_Credits_-1

> db.students.getIndexes()
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { Name: 1, Credits: -1 }, name: 'Name_1_Credits_-1' }
]
```

> Note that the "\_id\_" index which we see at first is created by default

Lastly, to drop all the Indexes, we use:

```javascript
> db.students.dropIndexes()
{
  nIndexesWas: 2,
  msg: 'non-_id indexes dropped for collection',
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1689588450, i: 3 }),
    signature: {
      hash: Binary(Buffer.from("1322555096a203195a231cd8e57621e4231957b5", "hex"), 0),
      keyId: Long("7200818374631227437")
    }
  },
  operationTime: Timestamp({ t: 1689588450, i: 3 })
}

> db.students.getIndexes()
[ { v: 2, key: { _id: 1 }, name: '_id_' } ]
```

---

Indexes are very helpful when we are dealing with large amounts of data. There are various Indexing strategies that we can use to find and retrieve the data more efficiently. Analyze your application's query patterns and identify the fields that are frequently queried. Creating indexes on these fields will significantly improve the performance of these queries. These Indexing strategies should be used to optimize the system for better results and faster data retrieval.

* **Avoid Over-Indexing**: While indexes boost query speed, they also consume storage space and add overhead during write operations. Avoid creating excessive indexes, as they might lead to unnecessary data duplication and increased maintenance costs.
    
* **Compound Indexes for Complex Queries**: For queries involving multiple fields, consider creating compound indexes to cover all the fields involved. A compound index can efficiently handle complex queries and avoid the need for separate indexes on individual fields.
    
* **Sorting and Aggregation**: Indexes can also boost the performance of sorting and aggregation operations. Creating indexes on fields used for sorting or aggregation stages can enhance the overall efficiency of these operations.
    
* **Indexing Arrays and Nested Fields**: When querying fields within arrays or nested documents, carefully choose indexes to ensure optimal query performance.
    
* **Regularly Monitor Index Usage**: Monitor the usage and effectiveness of indexes regularly. Use MongoDB's built-in tools like "explain" to analyze query execution plans and identify potential performance bottlenecks.
    

---

To conclude, Indexing is a fundamental technique that enhances query performance and overall database efficiency. By creating the right indexes and optimizing your database operations, you can achieve lightning-fast response times and a seamless user experience. Remember to implement indexing and performance optimization techniques based on your application's specific needs and query patterns. With careful planning and execution, you'll harness the true power of MongoDB for your projects.