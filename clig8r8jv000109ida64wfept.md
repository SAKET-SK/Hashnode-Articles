---
title: "Day 10: NodeJS - Body Parser Middleware"
datePublished: Sat Jun 03 2023 17:01:34 GMT+0000 (Coordinated Universal Time)
cuid: clig8r8jv000109ida64wfept
slug: day-10-nodejs-body-parser-middleware
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/TLpmjQ6t_9s/upload/b047f3a50370c74806f3a14ee7851d31.jpeg
tags: express, nodejs, backend, npm

---

Whenever we pass any kind of data from the client to the server or in our case from our NodeJS to REST-API, there will be a need to parse the contents of what is incoming as a request. For example, we are sending data in JSON format to the server in such large numbers. The server needs to know what data is being received. So a Middleware called "Body Parser" is used for this work.

In easy words, if we try to specify, some of the data which might be in JSON format is sometimes very hard to read as hard to interpret. So we try to use a body parser for it.

To install body-parser, type this command:

```plaintext
npm i body-parser
```

Now let's go back to code. Head over to your app.js file from the previous article and make these changes.

```javascript
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({extended: true}))
app.use(bodyParser.json({extended: true})) 
```

Whenever there will be an incoming request, the body parser will parse the whole request i.e. will read the whole request. It will read JSON data and present it to you in a readable format.

Make sure that the "app.use()" for body-parser should be above the rest of the declared things. For starters, check my updated "app.js".

```javascript
// We are going to use express in this file 
const express = require('express');
const app = express();

const productsRouter = require('./api/routes/products')

const morgan = require('morgan');

const bodyParser = require('body-parser')

// app.use((req,res)=>{
//     res.status(200).json({
//         msg: "This is a simple GET Request"
//     })
// })

app.use(bodyParser.urlencoded({extended: true}))
app.use(bodyParser.json({extended: true}))  
// This is what I meant, body-parser app.use() should be above all.
// Because it is a middleware, it should be the one to carry out parsing on data before moving to the main controller.
app.use(morgan("dev"))
app.use('/products', productsRouter)

module.exports = app

// Now to use this into server
```

In "products.js" we will check if the body-parser catches the data.

```javascript
router.post('/',(req,res)=>{

    // Body Parser will make following properties available
    console.log(req.body)
    console.log(req.body.name)
    console.log(req.body.price)

    res.status(200).json({
        msg: "This is a POST request for products"
    })
})
```

Take note that we have added these code lines in the POST operation earlier we implemented. Now onto testing.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685805192737/6b705c92-0585-4d86-8bd8-8494453e2626.png align="center")

Now we will try something different. We will create o JSON object for the product and use it. Head over to products.js to code. We have commented on the "req.body" 3 statements and wrote this code next to it.

```javascript
// Temp JSON object
    const tempProduct = {
        name: req.body.name,
        price: req.body.price
    }

res.status(200).json({
        msg: "This is a POST request for products",
        statusMsg: "Product Added",
        product: tempProduct
    })
```

This product object will be very helpful when we create our product and in turn, we store it in the database. For now, WE HAVEN'T saved it anywhere as of yet. We are just printing messages. Now lets test the working.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685811156020/2efea09e-38fd-4b77-b262-072b28da10cd.png align="center")

Thus we can see that by creating our JSON object too, we can achieve a desirable response.

NOTE: This body-parser middleware should be used before any other middleware you are using in the app.js

Thus we tried and worked on API using body-parser. Experiment around so that you may learn more about the functions.