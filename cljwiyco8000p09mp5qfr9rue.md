---
title: "Day 1: MongoDB - Introduction to MongoDB"
datePublished: Mon Jul 10 2023 07:11:03 GMT+0000 (Coordinated Universal Time)
cuid: cljwiyco8000p09mp5qfr9rue
slug: day-1-mongodb-introduction-to-mongodb
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cijiWIwsMB8/upload/7995795ea5225634bff3f739ceaaa2df.jpeg
tags: mean, mongodb, backend, mern

---

MongoDB is a powerful, open-source NoSQL document-oriented database. Unlike traditional relational databases, MongoDB stores data in flexible, JSON-like documents called BSON (Binary JSON), allowing for dynamic schema design. This flexibility enables developers to work with evolving data structures effortlessly, making MongoDB an excellent choice for projects with rapidly changing requirements.

Let us have a look at some of the key features of MongoDB:

* **Document-Oriented**: MongoDB's document-oriented approach means that data is stored in self-contained documents, akin to JSON objects. These documents can vary in structure and can be nested, providing a high degree of flexibility in data modeling.
    
* **Scalability and Performance**: MongoDB's architecture is designed for scalability, allowing for horizontal scaling across multiple servers. Its distributed nature ensures seamless handling of large volumes of data, making it suitable for high-performance applications.
    
* **Rich Query Language**: MongoDB offers a powerful and expressive query language that supports complex queries, filtering, sorting, and aggregations. Its query language provides excellent support for searching and retrieving data efficiently.
    
* **Indexing**: MongoDB supports various indexing techniques, including single-field indexes, compound indexes, and geospatial indexes. Indexing enhances query performance by facilitating faster data retrieval, especially when dealing with large datasets.
    
* **High Availability and Fault Tolerance**: MongoDB's replica sets provide automatic failover and data redundancy, ensuring high availability and fault tolerance. In the event of a primary node failure, a replica set automatically elects a new primary node, minimizing downtime and data loss.
    

---

MongoDB works on the concepts of Collections, and Documents, not the standard SQL terms which are used. Their meaning matches though, but the term name in SQL is different and in MongoDB it is different. We will have a look at some key components of MongoDB.

* **Collection** – This is a grouping of MongoDB documents. A collection is the equivalent of a table that is created in any other RDBMS such as Oracle or MS SQL.
    
* **Database** – This is a container for collections like in RDBMS wherein it is a container for tables. A MongoDB server can store multiple databases.
    
* **Document** \- A record in a MongoDB collection is basically called a document.
    
* **Field** \- A name-value pair in a document. While specifying the data in a document in JSON format, it is usually in a key-value pair.
    
* **JSON** – This is known as **JavaScript Object Notation**. This is a human-readable, plain text format for expressing structured data. JSON is currently supported in many programming languages.
    

A Basic Example of key-value pairs:

```json
{
    custID: 101,
    custName: "Gary",
    orderID: 359
}
```

This above is a document that has some data to be inserted. Inside this document, the three entries are called "Fields". As you can see, they follow a key-value pair format.

To summarize the different terms between MongoDB and SQL, have a look at below table:

| MongoDB terms | RDBMS terms | Difference |
| --- | --- | --- |
| Database | Database |  |
| Document | Tuple (Row) | In RDBMS, a row is simply a single, structured data item in a table. In MongoDB, the data is stored in the Documents. |
| Collection | Table (Collection of Rows & Columns) | In RDBMS, the table contains the columns and rows which are used to store the data. In MongoDB, there are Documents, which in turn have Fields and which in turn are in the format of "key-value" pairs. |
| Field | Column | In RDBMS, the Column denotes a set of data values. In MongoDB, they are known as Fields. |

---

Why do we want to choose MongoDB over RDBMS?

People will say that RDBMS is well managed because of the relationships we can see among tables and other data. However, the disadvantage of these tied-in relationships also adds some restrictions. In the case of MongoDB, there are no such things called relationships and as it is a distributed architecture, we may connect any server, we may apply indexing and we may retrieve the data as we want. RDBMS lacks this functionality when handling large amounts of data.

The popularity of MongoDB stems from its numerous advantages and benefits:

* **Flexibility**: MongoDB's flexible schema allows for agile development, accommodating changes in data structures without downtime or complex migrations.
    
* **Developer Productivity**: With its intuitive query language, seamless integration with popular programming languages, and extensive community support, MongoDB empowers developers to build applications more efficiently.
    
* **Scalability**: MongoDB's distributed architecture and sharding capabilities make it a suitable choice for handling massive amounts of data and scaling horizontally as your application grows.
    
* **Performance**: MongoDB's efficient indexing, data locality, and in-memory computing options contribute to its exceptional performance, enabling fast and responsive applications.
    
* **Community and Ecosystem**: MongoDB boasts a vibrant and active community, with extensive documentation, tutorials, and resources available. Additionally, MongoDB integrates with various frameworks, libraries, and tools, enhancing developer productivity.
    
* **The structure of a single object is clear**, as we specify all the structures inside the Document in JSON format, we are very well aware of its structure and thus can operate on data accordingly.
    
* MongoDB supports dynamic queries on documents.
    

---

Conclusion

In this introductory post, we've explored MongoDB, its key features, and the reasons behind its popularity in the world of databases. MongoDB's document-oriented approach, scalability, powerful query language, and flexibility make it an ideal choice for modern application development. Whether you're working on a personal project or a large-scale enterprise application, MongoDB offers the tools and capabilities to meet your evolving data storage and retrieval needs.

Stay tuned for the next post in our blog series, where I will guide you through the process of getting started with MongoDB and setting up your development environment. Happy learning!