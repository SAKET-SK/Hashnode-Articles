---
title: "Day 12: NodeJS - Connection to MongoDB using Mongoose"
datePublished: Mon Jul 03 2023 07:44:30 GMT+0000 (Coordinated Universal Time)
cuid: cljmk2e3p000j09mfb2vgfqdu
slug: day-12-nodejs-connection-to-mongodb-using-mongoose
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/602Cf9KHvS0/upload/63dd7acb12617c911416507e6726baa6.jpeg
tags: express, mongodb, nodejs

---

In the previous article, we saw the demonstration of MongoDB Atlas. We also used MongoDB Compass, a GUI application from which we can work on Databases. There are several other ways to connect to MongoDB. One of them is using Mongo Shell, which is essentially a Command line tool.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688136814204/a9d2f42b-77ce-4cf5-a713-694c062e0fd7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688137746365/1f1c7408-4619-4a23-aa94-67730709000b.png align="center")

As of now, I don't have MongoDB installed on my machine, so using the screenshot I obtained. Go to the MongoDB Atlas and later click on connect. As seen in the previous article, It gives us many options to connect. So select, Mongo Shell and install it. Later it will give you a link below, the same as it gave for MongoDB Compass. Use the link for execution in Mongo Shell as shown in the above image. It will ask you for a password. Once the password is matched, we are able to connect MongoDB through Mongo Shell successfully.

Another way of connecting MongoDB is by using our Node App. We can write some code in our JavaScript file or we can use a third-party library called Mongoose. We will be using its usage today.

So to summarize, these are the ways to create a connection to MongoDB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688137575379/a10889d5-7398-48b9-8132-94a755dc9a89.png align="center")

Using the Mongoose library, we can perform CRUD operations on the database. Mongoose is like a middleware between our Node App and MongoDB, so we can communicate and thus establish the connection.

To install Mongoose, type:

```plaintext
npm i mongoose
```

After installation, we need to establish a connection. So now go to MongoDB Atlas and select your cluster. Click on the "**Connect**" button and select the below option:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688142540803/5a6c68a0-7fb6-42d6-b255-f6a3e019da25.png align="center")

It will popup following screen:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688142620028/b2d8eba0-5195-4edb-af57-fa8132e9b757.png align="center")

As you can see, it is giving us a string, which we can copy into our code. We will create a new file by the name "mongoose-test.js". The contents of the file are as below:

```javascript
const mongoose = require('mongoose')
const uri = "mongodb+srv://saketkhopkar910:saket%40910@cluster0.sftltxg.mongodb.net/school"
// %40 in place of @ is because we get syntax error
mongoose.connect(uri)
.then(()=>{
    console.log("Connection Established")
})
```

This is the URI (Uniform Resource Identifier) that has been obtained as we discussed earlier. I have just added '/school' in front of URI as in my case, it is the database that I have created and I wish to establish the connection with it via Mongoose. So the above program will help us verify whether Mongoose is helping us or not.

If we execute the above code, we get:

```plaintext
> node ./mongoose-test.js
Connection Established
```

Now let us test the connection using the "mongoose.connect()" method. In our program, we are creating an API for performing CRUD operations on products. When we have to get the data from the server to the client or the client to the server, we need to define a Model. A Model is something that holds the data. To use Mongoose, we need to define this model, also known as Schema. A Schema is the structure of our data (which we want to read/store). In Typescript, if we want to create a student object, we need to define a structure called class. The same is with databases; we need to implement a model or schema for that. We need to tell Mongoose the structure of the object which is incoming so that Monggose will work accordingly.

Let us create a model named "Product". Refer the code snippet below.

```javascript
// Models are responsible for creating and reading documents from the underlying MongoDB database. 
// Here we are going to create a Model.

const mongoose = require('mongoose')
const Schema = mongoose.Schema

const productSchema = Schema({     // Structure required
    _id: Schema.Types.ObjectId,
    name: {type:String, require:true},
    price: {type:String, require:true}
})

module.exports = mongoose.model("Product", productSchema)   // "Product" is a model name
```

As you can see, we strictly need to specify to Mongoose the details of the schema. In the above code, we have created a Model. From now on, whenever you need to add data via Mongoose, we need to create an object of this Model, copy your incoming object values into it, and then we need to call it a save so that we can finally save it in the MongoDB Database.

Remember the products.js file we were working with in previous blogs up till the Body Parser concept? Well, we are going to use it now. We will refer to the part of the file which handles POST requests. The part where we will be changing the code is also marked in curly brackets.

```javascript
const Product = require('../model/product')
const mongoose = require('mongoose')
// Handle POST requests for product i.e. I want to create a new product {CREATE}
router.post('/',(req,res)=>{

    // Body Parser will make following properties available
    // console.log(req.body)
    // console.log(req.body.name)
    // console.log(req.body.price)

    // {PERFORMING MODEL MONGODB OPERAIONS HERE}
    // 1) Create an OBJECT of the MODEL
    const product = new Product({
        _id: new mongoose.Types.ObjectId(),
        name: req.body.name,
        price: req.body.price
    })

    // 2) Save this to MongoDB Database
    product.save()
    .then((res)=>{
        console.log(res)
    })
    .catch((err)=>{
        console.log(err)
    })

    res.status(200).json({
        msg: "This is a POST request for products",
        statusMsg: "Product Added",
        product: product
    })
})
```

But as of now, we only wrote the logic. We have not yet connected to the database. So to connect to the database, go to your app.js file and establish the connections there. Refer to my app.js file for reference.

```javascript
// We are going to use express in this file 
const express = require('express');
const app = express();
const productsRouter = require('./api/routes/products')
const morgan = require('morgan');
const bodyParser = require('body-parser')
const mongoose = require('mongoose')

// app.use((req,res)=>{
//     res.status(200).json({
//         msg: "This is a simple GET Request"
//     })
// })

app.use(bodyParser.urlencoded({extended: true}))
app.use(bodyParser.json({extended: true}))  

app.use(morgan("dev"))

const uri = "mongodb+srv://saketkhopkar910:saket%40910@cluster0.sftltxg.mongodb.net/school"
// %40 in place of @ as it causes error
mongoose.connect(uri)

app.use('/products', productsRouter)

module.exports = app

// Now to use this into server
```

Focus on the "URI" variable. To obtain a link for connection as usual, we will go to MongoDB Compass and obtain the link which we were using as URI in previous blogs. Now to test the POST call. I will go to my Thunder Client in VSCode now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688365521014/b4fe9e85-ecf1-4917-9c78-86db79111e27.png align="center")

As you can see, it has generated an ID and listed the product details in the response section. Thus indicating that our code works so far. Now to check if the data has been added to MongoDB, we need to go to MongoDB compass and refresh it once.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688365771826/ec60b96f-20a5-4f94-a414-f09e94ecb500.png align="center")

As you can see, our data has been successfully added to MongoDB. All this is possible thanks to "Mongoose". But the task is not yet complete as among the CRUD, we only implemented C (Create) i.e. we only wrote the data to the database. We will see the other operations in upcoming blogs.