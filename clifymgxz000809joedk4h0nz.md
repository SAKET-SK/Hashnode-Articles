---
title: "Day 9: NodeJS - Creating our own REST API"
datePublished: Sat Jun 03 2023 12:17:56 GMT+0000 (Coordinated Universal Time)
cuid: clifymgxz000809joedk4h0nz
slug: day-9-nodejs-creating-our-own-rest-api
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/NPFfuGST5YA/upload/a6d8da4f1e49c7f1477e37940cd45901.jpeg
tags: express, nodejs, backend

---

What's a REST API?

REST stands for "REpresentational State Transfer". Whenever a client wants to connect to the server, the client can do so with the help of these REST endpoints which are given by the server. The server provides REST endpoints so that the client is going to use them for connection.

An endpoint is exactly similar to Sockets which are present in our home. A client just connects to it to use further services. It is like a URL that represents a particular resource. Also, whenever we talk about REST API anytime, it is mostly about CRUD operations.

```plaintext
CRUD operations
    Create   -> POST
    Read     -> GET
    Update   -> PUT
    Delete   -> DELETE
```

Endpoint example:

```plaintext
 http://www.website.com/api/employees
 [All operations on this employees will use this endpoint]
```

Let us dive deep into detailed examples of CRUD operations using Endpoint. We will see what kind of data is read when it is entered in Endpoint and what response the server gives to it.

```plaintext
[GET EMPLOYEES (READ)]
Read :- http://www.website.com/api/employees
Response :- Data of all employees in JSON format
```

```plaintext
[GET A SINGLE EMPLOYEE]
Read :- http://www.website.com/api/employees/1001
Response :- Data of the specified employee with id = 1001 in JSON format
```

```plaintext
[POST AN EMPLOYEE (CREATE)]
Read :- http://www.website.com/api/employees &  
Pass JSON data (JSON payload; roughly means any data which is sent along with a request) to be added (in request body)
Response :-  Data of newly added employee in JSON format
```

```plaintext
[PUT AN EMPLOYEE (UPDATE)]
Read :- http://www.website.com/api/employees &  
Pass JSON data to be updated (in request body)
Response :- Data of updated employee in JSON format
```

```plaintext
[DELETE AN EMPLOYEE (DELETE)]
Read :- http://www.website.com/api/employees/1004
Response :- Deletes the employee with id = 1004
```

This is typically how a REST API works, or we can say it is implemented. Now we will try to create our REST API.

Create a new folder. I have done so and named the folder "rest-api".

We will install the following required modules: Install required modules (express,dotenv, and nodemon). Don't forget "npm init" before you install these modules.

Create a ".env" file as we created previously. Also, a JavaScript file in which we will do some code. Your folder structure should be like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685769340693/5296d3c9-c439-414d-be7d-a131e7ae9750.png align="center")

In server.js, we will try to see for starting purposes if our server is working properly or not.

```javascript
require(dotenv).config()   // So env variables are made avaialable using process keyword
const http = require('http');

const port = process.env.port;
const host = process.env.host;

const server = http.createServer((req,res)=>{
    if(req.url == '/'){
        res.write("Hello World")
        res.end()
    }
})

server.listen(port,()=>{
    console.log(`Server started at ${host}:${port}`)
})
```

Executing the above file gives us the output that our server works. Now we will try to push a request to the server using ExpressJS.

Create another file "app.js".

```javascript
// We are going to use express in this file 
const express = require('express');
const app = express();

app.use((req,res)=>{
    res.status(200).json({
        msg: "This is a simple GET Request"
    })
})

module.exports = app    //Exporting, so now to use this into server
```

Export it later to be used in the server. To use it on the server, we need to do the following changes.

```javascript
// Import the exported app module
const app = require('./app')
// Create the app
const server = http.createServer(app)
```

So have a look at the updated code of the server.js. The commented part is our old way we did it previously.

```javascript
require('dotenv').config()   // So env variables are made avaialable using process keyword
const http = require('http');

const port = process.env.port;
const host = process.env.host;

const app = require('./app')

// const server = http.createServer((req,res)=>{
//     if(req.url == '/'){
//         res.write("Hello World")
//         res.end()
//     }
// })

const server = http.createServer(app)

server.listen(port,()=>{
    console.log(`Server started at http://${host}:${port}`)
})
```

If we run it, we can see on the server that a request is generated. But we will not show this thing to users always.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685776215418/f4d400a1-012e-4cc0-a363-882807da526e.png align="center")

So there are two ways here to test our rest-API.

1. Install Thunder Client plugin for VS Code
    
2. Use POSTMAN
    

We will be using the first way to do so. My server in my case, is running on localhost:3333. Now I will be putting its link into Thunder Client to test it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685776592946/41a650bb-1e88-4be5-bc9e-3332bcfb02bc.png align="center")

Thus we can see that our rest-API works and is up and running. This was a demo. Now we will be actually implementing it. This is our target design.

```plaintext
/products                  /orders
GET    POST                GET      POST

/products/{id}           /orders/{id}
GET  PUT  DELETE         GET  PUT  DELETE
```

We will be using Mongo Atlas for storing our data for now. MongoDB is typically easy to learn as there are not many concepts as compared to SQL. We will be creating the database there and using it.

Before doing so, let us make sure that JavaScript code files are ready for server operation. We will be perofmring routing operations for products.

Create a folder named "api" followed by one more folder inside it as "routes". In routes folder, create products.js.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685789812288/44943f91-f97d-4df3-a32e-8dd7a60db1c1.png align="center")

Inside **products.js**, we are testing the handling of GET and POST requests.

```javascript
const express = require('express')
const router = express.Router()

// Handle GET requests for products
router.get('/',(req,res)=>{
    res.status(200).json({
        msg: "This is a GET request for products"
    })
})

// Handle POST requests for product
router.post('/',(req,res)=>{
    res.status(200).json({
        msg: "This is a POST request for products"
    })
})

module.exports = router
```

Here is our updated **server.js** file below. This is the server that we are revolving around. The change we did is to remove the old code which signifies the successful execution of the first GET request we saw earlier. That is because, the requests related to products, will be coming on different routes i.e. on the route "/products".

```javascript
require('dotenv').config()   // So env variables are made avaialable using process keyword
const http = require('http');

const port = process.env.port;
const host = process.env.host;

const app = require('./app')

// const server = http.createServer((req,res)=>{
//     if(req.url == '/'){
//         res.write("Hello World")
//         res.end()
//     }
// })

const server = http.createServer(app)

server.listen(port,()=>{
    console.log(`Server started at http://${host}:${port}`)
})
```

And lastly, here is our "app.js".

```javascript
// We are going to use express in this file 
const express = require('express');
const app = express();

const productsRouter = require('./api/routes/products')

// app.use((req,res)=>{
//     res.status(200).json({
//         msg: "This is a simple GET Request"
//     })
// })

app.use('/products', productsRouter)

module.exports = app

// Now to use this into server
```

We created a products Router and gave it the path to our products.js. Now let us understand the workflow between these three files, (products.js, server.js, app.js).

The first thing to understand is that our server.js, in easy summarization, our server is working well and well. Whenever there will come a request, we go into the app.js and inside the app, it is clearly stated that when the routing path is "/products" then use the product Router we created.

Now moving on to "products.js", if we send something via GET request, we will get the corresponding message.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685791040267/b6a90e78-458b-4b9a-ac42-8961927140f9.png align="center")

If we send something like this via POST, then we get this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685791134519/bc75a0db-54eb-4dce-a9db-78d1fe4f78a8.png align="center")

Suggesting that our api works.

Lets play with it more. This thing we did, is for multiple products. But what if we need to do the same thing for single product? This time, id of the product comes into picture. Later on we can also do PUT and DELETE opeartion on it as a result.

We need to following chunk of code into our products.js file.

```javascript
// Handle GET requests for SINGLE product
router.get('/:productId',(req,res)=>{
    res.status(200).json({
        msg: "This is a GET request for a SINGLE product"
    })
})
```

Now to test it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685792382720/d96e664c-b495-4db5-82e7-bef5ebc31e60.png align="center")

Let us try reading the id number we are passing. We can do so by making a slight change in our last code segment.

```javascript
// Handle GET requests for SINGLE product
router.get('/:productId',(req,res)=>{

    const id = req.params.productId

    if(id == 999){
        res.status(200).json({
            msg: "999 IS A SPECIAL ID"
        })
    }
    else{
        res.status(200).json({
            msg: "A REGULAR ID, NOTHING SPECIAL"
        })
    }  
})
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685792900896/43c9b9a7-f800-4985-881d-443ab32862d8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685792911620/b19e181d-518a-4d60-89e0-14eaf6e9d2c2.png align="center")

Now what remains is the operation of PUT (Update) and DELETE (Delete). Let us get back to our products.js for code.

```javascript
// Handle PUT requests i.e I want to make some changes to data of the product {UPDATE}
router.put('/:productId',(req,res)=>{

    const id = req.params.productId

    res.status(200).json({
        msg: "This is a PUT request for products",
        id: id
    })

})
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685793274161/99cc04f5-a237-492f-aae1-b0aa92c64f9c.png align="center")

Now to handle the DELETE.

```javascript
// Handle DELETE requests i.e I want to remove a product {DELETE}
router.delete('/:productId',(req,res)=>{

    const id = req.params.productId

    res.status(200).json({
        msg: "This is a DELETE request for products",
        id: id
    })

})
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685793463546/b528e92f-cbff-4d07-a502-cd535b94d88e.png align="center")

This is enough to test that all the functions we wrote in our rest-API are working perfectly. But still few things are missing here.

Let us a command which will start our application. To do so, follow these steps:

1. Go to package.json
    
2. Add the following command (under scripts): "start": "nodemon server.js"
    
3. Now we will just say "npm start" whenever we want to run our app
    

### Morgan Library

A Morgan Library is a third-party library that allows us to see the HTTP requests (to log the HTTP request information). To install it, just type:

```plaintext
npm i morgan
```

Later we need to make some changes to our "app.js".

```javascript
const morgan = require('morgan');
app.use(morgan("dev"))  // The dev is format in which we need our log output
```

Now we will just fire up some random GET and POST requests via Thunder Client and check out our console.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685794436512/c0789aab-0e3a-465e-bdf6-b718cc06fa62.png align="center")

As you can see, the request we fired up, gets logged onto our console. This is what a Morgan library does.

Thus we have successfully tested the working of our REST-API and hopefully we will implement it later on.