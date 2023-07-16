---
title: "Day 4: MongoDB - Unleashing the Power of Data Retrieval"
datePublished: Sun Jul 16 2023 17:34:31 GMT+0000 (Coordinated Universal Time)
cuid: clk5pv8fh000109mef3wg6zw1
slug: day-4-mongodb-unleashing-the-power-of-data-retrieval
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/gNHSuP7MZHw/upload/b6650caa3bec3e4d282df70b876b5748.jpeg
tags: mongodb, backend, databases

---

In this article, we'll explore the essentials of MongoDB querying, along with advanced techniques to make your data retrieval experience seamless and efficient. We will look into some queries with some examples to make things easy to understand.

Most of the power of querying in MongoDB revolves around the "find()" method. Using this find() method, we are successfully able to retrieve the data according to the criteria we need. We have already seen the use of "find()" in our previous MongoDB blog posts. But we will see them here again for a quick reminder.

```javascript
> db.students.find({})
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

These are all our Documents that are present in the Collection named "students". As you can see, we have passed an empty query object to the find() method. Though it is not needed compulsorily. We can just use "db.students.find()" as we have seen in previous blog posts.

Now let us move on to some more operations. Suppose we want to filter someone by name, then we use:

```javascript
> db.students.find({"Name":'KLM'})
[
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    id: 'TI004',
    Name: 'KLM',
    Credits: 8.2,
    City: 'Hyderabad'
  }
]
```

Similarly, we can filter any kind of record we want to, just we need to provide proper criteria to do so.

Now let us move on to the comparison operator. As we know that in programming, these operators are used to compare two values and return the result. There are similar operations of these operators in MongoDB too, the only thing is their appearance is a bit different.

```plaintext
'$eq' -> equal to
'$ne' -> not equal to
'$gt' -> greater than
'$lt' -> less than
```

Along with these, we can also use **'$and', '$or' and '$nor'** logical operators to specify the condition in more detail.

Suppose we want to find the data of students whose Credits are greater than 8.0 AND less than 9.0, then we use the following query.

```javascript
> db.students.find({$and:[{"Credits":{"$gte":8.0}},{"Credits":{"$lt":9.0}}]})
[
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    id: 'TI004',
    Name: 'KLM',
    Credits: 8.2,
    City: 'Hyderabad'
  }
]
```

There are also some nested data, which means data inside the data. Our data table currently does not have nested data, so we will be looking only at an example query. Consider a Field named "Address" which contains sub-fields: "City", "Street", "Pincode". If we want to retrieve that particular sub-field, say City. Then we use the following command:

```javascript
db.CollectionName.find({"Address.City":"New York"})
```

MongoDB also supports queries on the array. We have two records with an array (of Languages) inside them as seen before. Let us perform operations on them.

Suppose I want to retrieve the data record of the student who knows Python, then I will type:

```javascript
> db.students.find({"Languages":"Python"})
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

And what if I want to find students who know "Java" too?

```javascript
> db.students.find({"Languages":"Java"})
[
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

Now let us look into the examples of Projections and Sorting. In simple terms, you can control the data after its extraction.

Projections allow you to specify which fields to include or exclude in the query results. For example, if you want to retrieve only the "Name" and "Credits" fields from the above Collection, then:

```javascript
> db.students.find({},{"Name":1, "Credits":1})
[
  {
    _id: ObjectId("64b0ed7a88c9597b454ae8fa"),
    Name: 'ABC',
    Credits: 7.8
  },
  {
    _id: ObjectId("64b0eec888c9597b454ae900"),
    Name: 'XYZ',
    Credits: 7.3
  },
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    Name: 'KLM',
    Credits: 8.2
  },
  {
    _id: ObjectId("64b222bc0578e3efed8dc01a"),
    Name: 'MOD',
    Credits: 6.6
  },
  {
    _id: ObjectId("64b223100578e3efed8dc01b"),
    Name: 'ZKG',
    Credits: 6.6
  }
]
```

Sorting allows you to sort the output in ascending or descending order. Suppose I want to arrange the "Credits" values in descending order, then:

```javascript
> db.students.find().sort({"Credits":-1})
[
  {
    _id: ObjectId("64b0fe2588c9597b454ae902"),
    id: 'TI004',
    Name: 'KLM',
    Credits: 8.2,
    City: 'Hyderabad'
  },
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

Carefully observe the order. we can see that the output is arranged in descending order as we mentioned by typing "-1".

---

MongoDB's powerful querying capabilities empower you to retrieve and manipulate data effortlessly, making it an ideal choice for modern application development. Remember to practice and experiment with MongoDB querying to fully grasp its potential and maximize its benefits. Happy querying and see you in the next installment!