---
title: "Day 6: MongoDB - Simplifying Data Aggregation"
datePublished: Wed Jul 19 2023 08:34:22 GMT+0000 (Coordinated Universal Time)
cuid: clk9gw5aj000g0alf6qd3bhvi
slug: day-6-mongodb-simplifying-data-aggregation
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ofpr9Cw8Rj8/upload/c8959d4facc32a2e84a6b0653009a289.jpeg
tags: mongodb, backend, databases, aggregation

---

MongoDB's aggregation is a powerful framework that allows you to process, transform, and analyze data in the database. It provides a set of stages that can be chained together in a pipeline to perform various operations on the data. Data aggregation in MongoDB involves processing and transforming data to gain insights and answer complex questions. The aggregation framework facilitates this process by providing a set of stages that perform various data operations sequentially. By chaining these stages in the aggregation pipeline, developers can create sophisticated queries to meet diverse data analysis requirements.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689743464434/3844f18c-a071-49e4-bd66-9146277c0f67.webp align="center")

There might be other stages too other than the ones mentioned in the diagram. In this blog series, we will try to understand the important Aggregation Pipeline stages.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Miscellaneous TiP: For performing operations it is recommended to load sample datasets in your MongoDB Server via MongoDB Atlas. To do so, open up your Project dashboard. Click on the "..." button. Refer to the below image.</div>
</div>

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689659229574/324ff239-2692-4cd0-b99b-88447de67963.png align="center")

After clicking on that button, there will be the option of "Load Sample Datasets". Click on that option and wait until all the datasets are added. It will take 2-3 minutes and after the datasets are added, connect them with MongoDB Compass and MongoDB Shell to perform various operations.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689659413547/0d083a9c-fbf3-455d-b24e-82bc5aa01697.png align="center")

There are several aggregation queries that we will try to implement practically and these sample datasets will be very useful with that. As discussed earlier, the aggregation framework facilitates this process by providing a set of stages that perform various data operations sequentially. By chaining these stages in the aggregation pipeline, developers can create sophisticated queries to meet diverse data analysis requirements. We will be looking at these various stages.

---

### $group:

The $group stage groups documents together based on a specified key and accumulates data using aggregation functions like $sum, $avg, $min, $max, etc. This stage is perfect for performing calculations on grouped data. The $group aggregation stage in MongoDB is used to group documents together based on a specified key and perform aggregation operations on the grouped data. It is one of the most powerful and commonly used stages in the aggregation pipeline. Let's dive into the details of the $group stage with examples:

Let us insert some data.

```javascript
db.orders.insertMany([
  { _id: 1, product: "Laptop", quantity: 2, price: 800 },
  { _id: 2, product: "Mouse", quantity: 5, price: 25 },
  { _id: 3, product: "Keyboard", quantity: 3, price: 50 },
  { _id: 4, product: "Laptop", quantity: 1, price: 800 },
  { _id: 5, product: "Monitor", quantity: 2, price: 150 }
]);
```

This Collection "orders" will be automatically inserted in the "test" database. The syntax for "$group" is:

```javascript
{
    $group:{
    _id: <key>,
    <field1>: { <accumulator1> : <expression1> },
    <field2>: { <accumulator2> : <expression2> },
    ...
    }
}
```

The meaning of the above syntax is, the "\_id" parameter is the key which is usually a field name on which the data will be grouped. The "field" parameter is optional and you can include while querying, to store the expression result in simple terms. The "accumulator" is where your aggregation function will be. You may group the fields by "$sum", "$min", "$max", etc. The "expression" parameter is where you specify the criteria of what should be specified for aggregation.

Example 1 - Grouping and Calculating Total Sales:

Let's group the "orders" collection by the "product" field and calculate the total quantity sold for each product.

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      totalQuantitySold: { $sum: "$quantity" }
    }
  }
]);
```

```javascript
{ _id: 'Monitor', totalQuantitySold: 2 }
{ _id: 'Laptop', totalQuantitySold: 3 }
{ _id: 'Mouse', totalQuantitySold: 5 }
{ _id: 'Keyboard', totalQuantitySold: 3 }
```

Have a look at the output. In this example, we group orders by the "product" field using "$product" as the "\_id". The $sum aggregation function calculates the total quantity sold for each product, and the result will be an array of documents with the "\_id" representing the product and "totalQuantitySold" representing the calculated total.

You must be wondering why in spite of having 5 records, how come there are only 4 in output. Well, the Laptop occurs two times. The output effectively shows us the sales of products, even if they are repeated. Let us try to find the average price of the products.

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      averagePrice: { $avg: "$price" }
    }
  }
]);
```

```javascript
{ _id: 'Mouse', averagePrice: 25 }
{ _id: 'Laptop', averagePrice: 800 }
{ _id: 'Monitor', averagePrice: 150 }
{ _id: 'Keyboard', averagePrice: 50 }
```

Let us try to find the maximum price for each product.

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$product",
      maxPrice: { $max: "$price" }
    }
  }
]);
```

```javascript
{ _id: 'Mouse', maxPrice: 25 }
{ _id: 'Monitor', maxPrice: 150 }
{ _id: 'Keyboard', maxPrice: 50 }
{ _id: 'Keyboard', maxPrice: 50 }
```

The examples here are no big deal as we are operating with only small sets of data. But you will definitely see better results when working on larger sets of data.

---

### $match:

The $match aggregation stage in MongoDB is used to filter documents in the collection based on specified criteria. It allows you to select only those documents that match the given conditions and pass them to the next stage in the aggregation pipeline. Let's explore the $match stage in detail.

Consider the following sample data:

```javascript
db.employees.insertMany([
  { _id: 1, name: "John Doe", age: 32, department: "HR", salary: 50000 },
  { _id: 2, name: "Jane Smith", age: 28, department: "Engineering", salary: 60000 },
  { _id: 3, name: "Michael Johnson", age: 35, department: "Sales", salary: 55000 },
  { _id: 4, name: "Emily Brown", age: 27, department: "Engineering", salary: 65000 },
  { _id: 5, name: "William Lee", age: 30, department: "Marketing", salary: 48000 }
]);
```

The syntax of $match is pretty similar to the $group:

```javascript
{
  $match: {
    <field1>: <expression1>,
    <field2>: <expression2>,
    ...
  }
}
```

Let us understand the $match stage with an example:

Example 1 - Matching Documents with a Specific Condition: Let's find employees who are in the "Engineering" department.

```javascript
db.employees.aggregate([
  {
    $match: {
      department: "Engineering"
    }
  }
]);
```

```javascript
{ _id: 2, name: 'Jane Smith', age: 28, department: 'Engineering', salary: 60000 }
{ _id: 4, name: 'Emily Brown', age: 27, department: 'Engineering', salary: 65000 }
```

Example 2 - Matching Documents with Multiple Conditions: Let's find employees who are in the "Engineering" department and have a salary greater than 60000.

```javascript
db.employees.aggregate([
  {
    $match: {
      department: "Engineering",
      salary: { $gt: 60000 }
    }
  }
]);
```

```javascript
{ _id: 4, name: 'Emily Brown', age: 27, department: 'Engineering', salary: 65000 }
```

Let us try something different now:

Example 3 - Matching Documents with Regular Expression: Let's find employees whose names start with "J".

```javascript
db.employees.aggregate([
  {
    $match: {
      name: /^J/
    }
  }
]);
```

```javascript
{ _id: 1, name: 'John Doe', age: 32, department: 'HR', salary: 50000 }
{ _id: 2, name: 'Jane Smith', age: 28, department: 'Engineering', salary: 60000 }
```

This above example uses regular expressions. With the help of regular expressions, we can write more powerful queries and get the result we want.

---

### $project:

This stage simply means how you want to show your data. In simple terms, the $project aggregation stage in MongoDB is used to shape the output of the aggregation by specifying which fields to include or exclude, creating new fields, and renaming existing ones. It allows you to reshape the documents in the collection and control what data is passed to the next stage in the aggregation pipeline.

```javascript
db.students.insertMany([
  { _id: 1, name: "John Doe", age: 20, subject: "Mathematics", score: 90 },
  { _id: 2, name: "Jane Smith", age: 22, subject: "Science", score: 85 },
  { _id: 3, name: "Michael Johnson", age: 21, subject: "History", score: 78 },
  { _id: 4, name: "Emily Brown", age: 23, subject: "English", score: 92 },
  { _id: 5, name: "William Lee", age: 20, subject: "Mathematics", score: 88 }
]);
```

We usually use "0" and "1" in the $project stage to indicate not to show the field and to show the field respectively.

Example 1 - Including Specific Fields in the Output: Let's project only the "name" and "subject" fields in the output.

```javascript
db.students.aggregate([
  {
    $project: {
      name: 1,
      subject: 1
    }
  }
]);
```

```javascript
{ _id: 1, name: 'John Doe', subject: 'Mathematics' }
{ _id: 2, name: 'Jane Smith', subject: 'Science' }
{ _id: 3, name: 'Michael Johnson', subject: 'History' }
{ _id: 4, name: 'Emily Brown', subject: 'English' }
{ _id: 5, name: 'William Lee', subject: 'Mathematics' }
```

In this example, the $project stage includes only the "name" and "subject" fields in the output, and all other fields will be excluded.

Example 2 - Excluding Specific Fields from the Output: Let's project all fields except the "score" field in the output.

```javascript
db.students.aggregate([
  {
    $project: {
      score: 0
    }
  }
]);
```

```javascript
{ _id: 1, name: 'John Doe', age: 20, subject: 'Mathematics' }
{ _id: 2, name: 'Jane Smith', age: 22, subject: 'Science' }
{ _id: 3, name: 'Michael Johnson', age: 21, subject: 'History' }
{ _id: 4, name: 'Emily Brown', age: 23, subject: 'English' }
{ _id: 5, name: 'William Lee', age: 20, subject: 'Mathematics' }
```

Now let us perform an advanced operation.

Example 3 - Creating Calculated Fields: Let's create a new field "ageGroup" based on the age of the students.

```javascript
db.students.aggregate([
  {
    $project: {
      name: 1,
      age: 1,
      ageGroup: {
        $cond: {
          if: { $gte: ["$age", 21] },
          then: "Adult",
          else: "Minor"
        }
      }
    }
  }
]);
```

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Remember that $cond is like a ternary operator. If the expression condition specified inside the $cond is true, then it returns the first true value and if false then the second value is for the false condition.</div>
</div>

```javascript
{ _id: 1, name: 'John Doe', age: 20, ageGroup: 'Minor' }
{ _id: 2, name: 'Jane Smith', age: 22, ageGroup: 'Adult' }
{ _id: 3, name: 'Michael Johnson', age: 21, ageGroup: 'Adult' }
{ _id: 4, name: 'Emily Brown', age: 23, ageGroup: 'Adult' }
{ _id: 5, name: 'William Lee', age: 20, ageGroup: 'Minor' }
```

In this example, the $project stage includes the "name" and "age" fields in the output. It also creates a new field "ageGroup" based on a conditional expression. If the age is greater than or equal to 21, the value of "ageGroup" will be "Adult"; otherwise, it will be "Minor".

---

### $sort:

The $sort aggregation stage in MongoDB is used to sort the documents in the collection based on one or more fields. It allows you to control the order of the output documents in the aggregation pipeline.

Consider this sample data:

```javascript
db.books.insertMany([
  { _id: 1, title: "The Great Gatsby", author: "F. Scott Fitzgerald", genre: "Entertainment", price: 25 },
  { _id: 2, title: "To Kill a Mockingbird", author: "Harper Lee", genre: "Fiction", price: 20 },
  { _id: 3, title: "1984", author: "George Orwell", genre: "Histroy", price: 18 },
  { _id: 4, title: "Pride and Prejudice", author: "Jane Austen", genre: "Fiction", price: 22 },
  { _id: 5, title: "The Catcher in the Rye", author: "J.D. Salinger", genre: "Fiction", price: 15 }
]);
```

For the $sort aggregation stage, we use "1" and "-1", meaning output in ascending order and descending order respectively.

Example 1 - Sorting in Ascending Order: Let's sort the "books" collection by the "price" field in ascending order.

```javascript
db.books.aggregate([
  {
    $sort: {
      price: 1
    }
  }
]);
```

```javascript
{ _id: 5, title: 'The Catcher in the Rye', author: 'J.D. Salinger', genre: 'Fiction', price: 15 }
{ _id: 3, title: '1984', author: 'George Orwell', genre: 'History', price: 18 }
{ _id: 2, title: 'To Kill a Mockingbird', author: 'Harper Lee', genre: 'Fiction', price: 20 }
{ _id: 4, title: 'Pride and Prejudice', author: 'Jane Austen', genre: 'Fiction', price: 22 }
{ _id: 1, title: 'The Great Gatsby', author: 'F. Scott Fitzgerald', genre: 'Entertainment', price: 25 }
```

Have a look at the price section. All the documents are arranged in ascending order with respect to the "price" field.

Example 2 - Sorting by Multiple Fields: Let's sort the "books" collection first by the "title" field in ascending order and then by the "price" field in descending order.

```javascript
db.books.aggregate([
  {
    $sort: {
      genre: 1,
      price: -1
    }
  }
]);
```

```javascript
{ _id: 1, title: 'The Great Gatsby', author: 'F. Scott Fitzgerald', genre: 'Entertainment', price: 25 }
{ _id: 4, title: 'Pride and Prejudice', author: 'Jane Austen', genre: 'Fiction', price: 22 }
{ _id: 2, title: 'To Kill a Mockingbird', author: 'Harper Lee', genre: 'Fiction', price: 20 }
{ _id: 5, title: 'The Catcher in the Rye', author: 'J.D. Salinger', genre: 'Fiction', price: 15 }
{ _id: 3, title: '1984', author: 'George Orwell', genre: 'History', price: 18 }
```

As you can see, it has arranged the documents in ascending order with respect to the "genre" field. When the "genre" field shares the same value, the descending order set by price comes into effect.

In this example, the $sort stage sorts the documents in the "books" collection first by the "genre" field in ascending order. For books with the same genre, it sorts them by the "price" field in descending order.

---

### $skip:

The $skip aggregation stage in MongoDB is used to skip a specified number of documents in the collection before passing the remaining documents to the next stage in the aggregation pipeline. It is useful for implementing pagination and skipping a certain number of documents from the beginning of the result set.

This is the sample data we will be using:

```javascript
db.employees.insertMany([
  { _id: 1, name: "John Doe", age: 32, department: "HR", salary: 50000 },
  { _id: 2, name: "Jane Smith", age: 28, department: "Engineering", salary: 60000 },
  { _id: 3, name: "Michael Johnson", age: 35, department: "Sales", salary: 55000 },
  { _id: 4, name: "Emily Brown", age: 27, department: "Engineering", salary: 65000 },
  { _id: 5, name: "William Lee", age: 30, department: "Marketing", salary: 48000 }
]);
```

The syntax of $skip is rather simple:

```javascript
{
  $skip: <num>
}
```

The &lt;num&gt; indicates the number of documents to skip.

Example 1 - Skipping a Specified Number of Documents: Let's skip the first two documents in the "employees" collection and return the rest.

```javascript
db.employees.aggregate([
  {
    $skip: 2
  }
]);
```

```javascript
{ _id: 3, name: 'Michael Johnson', age: 35, department: 'Sales', salary: 55000 }
{ _id: 4, name: 'Emily Brown', age: 27, department: 'Engineering', salary: 65000 }
{ _id: 5, name: 'William Lee', age: 30, department: 'Marketing', salary: 48000 }
```

Example 2 - Implementing with $limit stage: Let's implement pagination and return to the second page of results, where each page contains two documents.

```javascript
db.employees.aggregate([
  {
    $skip: 2
  },
  {
    $limit: 2
  }
]);
```

```javascript
{ _id: 3, name: 'Michael Johnson', age: 35, department: 'Sales', salary: 55000 }
{ _id: 4, name: 'Emily Brown', age: 27, department: 'Engineering', salary: 65000 }
```

Example 3 - Combining $skip with Other Stages: Let's sort the "employees" collection by the "salary" field in descending order, skip the first two highest-paid employees, and return the remaining employees.

```javascript
db.employees.aggregate([
  {
    $sort: {
      salary: -1
    }
  },
  {
    $skip: 2
  }
]);
```

```javascript
{ _id: 3, name: 'Michael Johnson', age: 35, department: 'Sales', salary: 55000 }
{ _id: 1, name: "John Doe", age: 32, department: "HR", salary: 50000 }
{ _id: 5, name: 'William Lee', age: 30, department: 'Marketing', salary: 48000 }
```

In this example, we first use the $sort stage to sort the documents by the "salary" field in descending order. Then, we use the $skip stage to skip the first two highest-paid employees and return the remaining employees in the result set.

---

### $limit:

The $limit aggregation stage in MongoDB is used to limit the number of documents that are passed to the next stage in the aggregation pipeline. It allows you to control the size of the result set and retrieve only a specific number of documents from the collection.

```javascript
db.products.insertMany([
  { _id: 1, name: "Laptop", category: "Electronics", price: 800 },
  { _id: 2, name: "Smartphone", category: "Electronics", price: 500 },
  { _id: 3, name: "Headphones", category: "Electronics", price: 100 },
  { _id: 4, name: "Shirt", category: "Clothing", price: 25 },
  { _id: 5, name: "Jeans", category: "Clothing", price: 40 }
]);
```

The syntax of limit is exactly similar to $skip:

```javascript
{
  $skip: <num>
}
```

But here, the &lt;num&gt; has a different meaning. It represents the maximum number of documents to return. Note that, this number should be strictly positive.

Example 1 - Limiting the Result Set: Let's retrieve the first three documents from the "products" collection.

```javascript
db.products.aggregate([
  {
    $limit: 3
  }
]);
```

```javascript
{ _id: 1, name: "Laptop", category: "Electronics", price: 800 }
{ _id: 2, name: "Smartphone", category: "Electronics", price: 500 }
{ _id: 3, name: "Headphones", category: "Electronics", price: 100 }
```

Example 2 - Combining $limit with Other Stages: Let's sort the "products" collection by the "price" field in descending order and return only the top two most expensive products.

```javascript
db.products.aggregate([
  {
    $sort: {
      price: -1
    }
  },
  {
    $limit: 2
  }
]);
```

```javascript
{ _id: 1, name: "Laptop", category: "Electronics", price: 800 }
{ _id: 2, name: "Smartphone", category: "Electronics", price: 500 }
```

In this example, we first use the $sort stage to sort the documents by the "price" field in descending order. Then, we use the $limit stage to limit the output to two documents. This effectively returns the two most expensive products.

---

### $unwind:

The $unwind aggregation stage in MongoDB is used to deconstruct an array field within a document and output one document for each element in the array. It is particularly useful when you have documents with nested arrays and want to perform operations on the individual elements of those arrays.

```javascript
db.cust_orders.insertMany([
  { _id: 1, customer: "John Doe", products: ["Laptop", "Mouse", "Keyboard"], totalAmount: 1800 },
  { _id: 2, customer: "Jane Smith", products: ["Smartphone", "Headphones"], totalAmount: 700 },
  { _id: 3, customer: "Michael Johnson", products: ["Laptop", "Printer", "Monitor"], totalAmount: 2500 },
  { _id: 4, customer: "Emily Brown", products: ["Tablet"], totalAmount: 400 }
]);
```

The syntax of $unwind is rather complex. But don't worry, we will dive deep into the explanations in simple terms and examples:

```javascript
{
  $unwind: {
    path: <arrayField>,
    includeArrayIndex: <string>,
    preserveNullAndEmptyArrays: <boolean>
  }
}
```

The &lt;path&gt; is the field that contains the array to unwind. It must be specified using dot notation if the array is within a nested document.

The &lt;includeArrayIndex&gt; is an optional field. This parameter allows you to include the array index as a new field in the output documents. If specified, the value should be a string that represents the new field name.

The &lt;preserveNullAndEmptyArrays&gt; is also an optional field. It takes boolean values. This parameter is a boolean that controls whether to include documents with empty arrays or null values after unwinding.

Example 1 - Unwinding an Array Field: Let's unwind the "products" array field in the "orders" collection.

```javascript
db.cust_orders.aggregate([
  {
    $unwind: "$products"
  }
]);
```

```javascript
{ _id: 1, customer: 'John Doe', products: 'Laptop', totalAmount: 1800 }
{ _id: 1, customer: 'John Doe', products: 'Mouse', totalAmount: 1800 }
{ _id: 1, customer: 'John Doe', products: 'Keyboard', totalAmount: 1800 }
{ _id: 2, customer: 'Jane Smith', products: 'Smartphone', totalAmount: 700 }
{ _id: 2, customer: 'Jane Smith', products: 'Headphones', totalAmount: 700 }
{ _id: 3, customer: 'Michael Johnson', products: 'Laptop', totalAmount: 2500 }
{ _id: 3, customer: 'Michael Johnson', products: 'Printer', totalAmount: 2500 }
{ _id: 3, customer: 'Michael Johnson', products: 'Monitor', totalAmount: 2500 }
{ _id: 4, customer: 'Emily Brown', products: 'Tablet', totalAmount: 400 }
```

In this example, the $unwind stage deconstructs the "products" array field in each document, and for each element in the array, it outputs a separate document with the same fields except for the "products" field, which now contains a single product.

Example 2 - Including Array Index in Output: Let's unwind the "products" array field and include the array index as a new field called "productIndex."

```javascript
db.cust_orders.aggregate([
  {
    $unwind: {
      path: "$products",
      includeArrayIndex: "productIndex"
    }
  }
]);
```

```javascript
{ _id: 1, customer: 'John Doe', products: 'Laptop', totalAmount: 1800, productIndex: 0 }
{ _id: 1, customer: 'John Doe', products: 'Mouse', totalAmount: 1800, productIndex: 1 }
{ _id: 1, customer: 'John Doe', products: 'Keyboard', totalAmount: 1800, productIndex: 2 }
{ _id: 2, customer: 'Jane Smith', products: 'Smartphone', totalAmount: 700, productIndex: 0 }
{ _id: 2, customer: 'Jane Smith', products: 'Headphones', totalAmount: 700, productIndex: 1 }
{ _id: 3, customer: 'Michael Johnson', products: 'Laptop', totalAmount: 2500, productIndex: 0 }
{ _id: 3, customer: 'Michael Johnson', products: 'Printer', totalAmount: 2500, productIndex: 1 }
{ _id: 3, customer: 'Michael Johnson', products: 'Monitor', totalAmount: 2500, productIndex: 2 }
{ _id: 4, customer: 'Emily Brown', products: 'Tablet', totalAmount: 400, productIndex: 0 }
```

In this example, the $unwind stage deconstructs the "products" array field and preserves documents with empty arrays or null values after unwinding.

Now for our next example, we will be needing an empty array. So I am adding a new record:

```javascript
db.cust_orders.insert({"_id":5,"customer":"Harvey",products:[],totalAmount:0})
```

Example 3 - Handling Empty Arrays: Let's unwind the "products" array field and preserve documents with empty arrays.

```javascript
{ _id: 1, customer: 'John Doe', products: 'Laptop', totalAmount: 1800 }
{ _id: 1, customer: 'John Doe', products: 'Mouse', totalAmount: 1800 }
{ _id: 1, customer: 'John Doe', products: 'Keyboard', totalAmount: 1800 }
{ _id: 2, customer: 'Jane Smith', products: 'Smartphone', totalAmount: 700 }
{ _id: 2, customer: 'Jane Smith', products: 'Headphones', totalAmount: 700 }
{ _id: 3, customer: 'Michael Johnson', products: 'Laptop', totalAmount: 2500 }
{ _id: 3, customer: 'Michael Johnson', products: 'Printer', totalAmount: 2500 }
{ _id: 3, customer: 'Michael Johnson', products: 'Monitor', totalAmount: 2500 }
{ _id: 4, customer: 'Emily Brown', products: 'Tablet', totalAmount: 400 }
{ _id: 5, customer: 'Harvey', totalAmount: 0 }
```

Previously, we haven't used this &lt;preserveNullAndEmptyValues&gt;, then the fifth record would get ignored. However, after setting this property to true, it prserves the values even if they are null and empty. The $unwind stage deconstructs the "products" array field and preserves documents with empty arrays or null values after unwinding.

---

These are just a few examples of MongoDB aggregation queries. The aggregation framework provides a wide range of stages and aggregation functions, allowing you to perform complex data manipulations and analysis directly in the database. It's a powerful tool for gaining valuable insights from your MongoDB data. That's all to conclude. Till then keep experimenting and keep learning.