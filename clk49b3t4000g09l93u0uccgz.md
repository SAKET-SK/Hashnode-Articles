---
title: "Day 3: MongoDB - Data Modelling and more queries"
datePublished: Sat Jul 15 2023 17:03:12 GMT+0000 (Coordinated Universal Time)
cuid: clk49b3t4000g09l93u0uccgz
slug: day-3-mongodb-data-modelling-and-more-queries
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/oyXis2kALVg/upload/8718e63506b3aa2a72fa43c202a07143.jpeg
tags: mongodb, backend

---

When we operate MongoDB, its primary function is to store the data. The data is in the form of JSON documents. Where there is data, there is a need to define the structure of data and thus store it according to the defined structure. This is because sometimes the Documents in various Collections may vary in terms of the data they hold.

For example, let us insert some data in our previous collection but with a new column:

```javascript
db.students.insert({"id":"TI003","Name":"MOD","Credits":6.6,"City":"Delhi","Languages":["C","C++","Java"]})
```

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find();
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

The structure of the last two documents is a bit different than the above three others. The Languages column is newly added and thus its data structure is a bit changed than the others. Where there is a talk of architecture, there is a concept of the data model. In short data modeling means defining the structure of data, storing the data and later performing the operations on that data. Let us look into the types of data modeling in MongoDB:

1. Embedded Data Model
    
2. Normalized Data Model
    

---

Embedded Data Modeling in MongoDB involves storing related data within a single document. This approach is suitable for one-to-one and one-to-many relationships.

Suppose we are building an e-commerce application, and we have two entities: "User" and "Order". Each user can have multiple orders. Instead of storing orders in a separate collection, we can embed them within the User document.

```javascript
// User Document
{
  _id: ObjectId("60e9c7b5e5732706a496a08d"),
  name: "John Doe",
  email: "johndoe@example.com",
  orders: [
    {
      _id: ObjectId("60e9c7b5e5732706a496a08e"),
      orderDate: ISODate("2021-07-10T09:30:00Z"),
      totalAmount: 50.25,
      products: [
        {
          name: "Product 1",
          price: 10.99,
          quantity: 2
        },
        {
          name: "Product 2",
          price: 14.99,
          quantity: 1
        }
      ]
    },
    {
      _id: ObjectId("60e9c7b5e5732706a496a08f"),
      orderDate: ISODate("2021-07-12T15:45:00Z"),
      totalAmount: 35.50,
      products: [
        {
          name: "Product 3",
          price: 17.75,
          quantity: 2
        }
      ]
    }
  ]
}
```

In the above example, we have a User document that contains an array field called "orders". Each order is represented as an embedded document within the "orders" array. Each order document contains order details like orderDate, totalAmount, and an array of products.

This embedded data model allows us to retrieve all the order details for a user in a single database query. It eliminates the need for joins or separate queries to retrieve orders associated with a user, resulting in improved read performance.

Embedded data modeling in MongoDB offers simplicity and efficiency for scenarios where related data is closely associated. However, it's important to consider the trade-offs, such as document size limits and potential data duplication, depending on the specific requirements of your application.

Remember to adjust your data model based on the nature of your data, access patterns, and scalability requirements.

---

Normalized Data Modeling in MongoDB involves storing related data in separate collections and using references to establish relationships between them. This approach is suitable for many-to-many relationships or scenarios where data needs to be updated in multiple places.

Continuing with our e-commerce application example, let's introduce a new entity called "Product" and model the relationship between users, orders, and products using normalized data modeling.

Here's an example data model in MongoDB using normalized data modeling:

```javascript
// User Document
{
  _id: ObjectId("60e9c7b5e5732706a496a08d"),
  name: "John Doe",
  email: "johndoe@example.com"
}

// Product Document
{
  _id: ObjectId("60e9c7b5e5732706a496a090"),
  name: "Product 1",
  price: 10.99
}

// Order Document
{
  _id: ObjectId("60e9c7b5e5732706a496a08e"),
  user: ObjectId("60e9c7b5e5732706a496a08d"),
  orderDate: ISODate("2021-07-10T09:30:00Z"),
  totalAmount: 50.25,
  products: [
    { product: ObjectId("60e9c7b5e5732706a496a090"), quantity: 2 },
    { product: ObjectId("60e9c7b5e5732706a496a091"), quantity: 1 }
  ]
}
```

In the above example, we have three separate collections: "users", "products", and "orders". The "orders" collection includes a reference to the user who placed the order and an array of product references along with their corresponding quantities.

By storing related data in separate collections and using references, we achieve data normalization. This approach ensures data consistency and reduces data redundancy.

Normalized data modeling allows for greater flexibility when working with complex relationships and enables more efficient updates and modifications. However, it might require additional queries or aggregation stages for retrieving related data, resulting in slightly increased complexity compared to embedded data modeling.

Remember to carefully consider your application's data access patterns, scalability requirements, and the need for data consistency when choosing between embedded and normalized data modeling in MongoDB.

---

Let us look to execute more MongoDB queries:

We all know about our data from a collection named Students. Let us perform operations on it.

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find();
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

Suppose we want to find someone by some criteria. Then we use the below command:

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find({"id":"TI002"})
[
  {
    _id: ObjectId("64b0eec888c9597b454ae900"),
    id: 'TI002',
    Name: 'XYZ',
    Credits: 7.3,
    City: 'Kolkata'
  }
]
```

We can also specify more than one criteria for filtering:

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find({"id":"TI002","Name":"XYZ"})
[
  {
    _id: ObjectId("64b0eec888c9597b454ae900"),
    id: 'TI002',
    Name: 'XYZ',
    Credits: 7.3,
    City: 'Kolkata'
  }
]
```

If we want to return only some specific records, then:

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find({},{"id":1,"Name":1})
[
  {
    _id: ObjectId("64b0ed7a88c9597b454ae8fa"),
    id: 'TI001',
    Name: 'ABC'
  },
  {
    _id: ObjectId("64b0eec888c9597b454ae900"),
    id: 'TI002',
    Name: 'XYZ'
  },
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    id: 'TI004',
    Name: 'KLM'
  },
  {
    _id: ObjectId("64b222bc0578e3efed8dc01a"),
    id: 'TI003',
    Name: 'MOD'
  },
  {
    _id: ObjectId("64b223100578e3efed8dc01b"),
    id: 'TI005',
    Name: 'ZKG'
  }
]
```

In another query we will be looking for a student who knows Python language, then:

```javascript
Atlas atlas-m4zqrn-shard-0 [primary] school> db.students.find({"Languages":"Python"})
[
  {
    _id: ObjectId("64b222bc0578e3efed8dc01a"),
    id: 'TI003',
    Name: 'MOD',
    Credits: 6.6,
    City: 'Delhi',
    Languages: [ 'C', 'C++', 'Java', 'Python' ]
  }
]
```

In such cases, there are a lot of queries to experiment with. We will be looking at more queries of MongoDB in detailed in upcoming blog posts.