---
title: "Day 13: NodeJS - CRUD operations via NodeJS to MongoDB"
datePublished: Tue Jul 04 2023 08:18:10 GMT+0000 (Coordinated Universal Time)
cuid: cljo0pjl3000109mjg9vs5xkf
slug: day-13-nodejs-crud-operations-via-nodejs-to-mongodb
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/yxQ9HlwmFLY/upload/6d4f9e55ee339378afa3d8cea6ba0773.jpeg
tags: mongodb, nodejs, backend, mongoose

---

In our last blog, we saw how we implemented the C of CRUD via NodeJS to MongoDB. In simple terms, we successfully executed the code of our API which we created previously. Later we handled the POST call, connected with MongoDB Database, and stored the data there. In this blog, we will be doing the rest of the remaining CRUD operations via NodeJS to MongoDB.

But before that, we will take a look at another fundamental concept: Async & Await.

By using the "Async" function, there is no need to give callbacks. Recently we used the callbacks as:

```javascript
product.save()
    .then((res)=>{        // callback function
        console.log(res)
    })
    .catch((err)=>{
        console.log(err)
    })
```

When we mark any function as "async", it means that it will return some "promises". Now what does a "promise" mean? Where there is a promise, there is a callback. "Async" allows us to use the "await" keyword inside the function. It is important to note that the "await" keyword can be used only in the "async" function. The main purpose of the "await" keyword is used to pause the execution of the "async" function until a promise is resolved or rejected. It waits for the promise to settle and then resumes the execution of the function. While the function is waiting for the promise to resolve, it can perform other tasks or wait for other promises without blocking the execution.

Now let us see the practical demonstration. I have created a folder named "controller" and it contained the "product.model.js" file. Below are its contents. Refer to the code snippet below:

```javascript
// We will handle things here with ASYNC/AWAIT

const mongoose = require('mongoose')
const Product = require('../model/product')

exports.createProduct = async (req,res) => {
    try{
        // Create product object
        const product = new Product({
            _id: new mongoose.Types.ObjectId(),
            name: req.body.name,
            price: req.body.price
        })

        const data = await product.save()    // Response of await will be stores in the constant "data"
        // Roughly above statement means, call product.save and AWAIT until it returns!!!

        // If code execution reaches THIS POINT, that means product.save was a successful.
        res.status(200).json({
            msg: "Product Created Succesfully",
            product: data                          // "product" is the "data" that you receive from here
        })


    } catch(err){                // Catch the error in case AWAIT fails.
        res.status(200).json({
            code: 200,
            msg: "Something went wrong",
            err: err                  // Send error object back
        })
    }
}
```

Now we will go back to our "products.js" present in the "routes" folder as created earlier. Instead of the callbacks, we will comment out the whole code which handles the POST request and simply write:

```javascript
router.post('/', productController.createProduct)
```

Using "async/await" makes the code appear more synchronous and easier to read compared to chaining multiple ".then()" callbacks. It provides a more intuitive way to handle asynchronous operations and reduces the "callback hell" problem.

To summarize the above explanation, this is what we did:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688397194112/b394c2af-53a5-4203-a525-01af624fa716.png align="center")

At this point, we implemented C from CRUD. We will be using this async/await approach for further operations.

Next, we will implement the R from CRUD. We will try to read the documents (in the Database language they are known as Records). In MongoDB to do so, we use the "find()" method. By using a similar async/await format, we can write the method as:

```javascript
// R from CRUD [READ]
exports.getProducts = async(req, res) => {
    try{
        const products = await Product.find();    // Get all the documents from the collection
        res.status(200).json({
            msg: "All Documents Fetched Successfully",
            data: products                // "data" is the list of "products" you will receive from here              
        })

    }catch(err){
        res.status(501).json({
            code: 501,
            msg: "Something went wrong",
            err: err                  // Send error object back
        })
    }
}
```

In our file "routes/products.js", we will write:

```javascript
router.get('/', productController.getProducts)
```

Now let us test it with our Thunder Client.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688448926216/a9d793b7-fc83-4b4f-b295-5d48cf6ca78a.png align="center")

As you can see, we get all our records successfully. Thus we have implemented "C from CRUD" and "R from CRUD". But what if we only want the data of the product with a specific ID? Let's look into that.

```javascript
// R from CRUD [READ] but only read specific document not all!!
exports.getProductByID = async(req, res) => {
    try{
        const product = await Product.findById(req.params.productId);    // Get single document from the collection
        res.status(200).json({
            msg: "Document Fetched Successfully",
            data: product                // "data" is the "product" you will receive from here              
        })

    }catch(err){
        res.status(501).json({
            code: 501,
            msg: "Something went wrong",
            err: err                  // Send error object back
        })
    }
}
```

As always, inside our "routes/products.js" file, we write:

```javascript
router.get('/:productId',productController.getProductByID)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688450880440/47184f7b-a834-4372-b64f-13d003d71529.png align="center")

Now let us move to implement the "U of CRUD", meaning the Update functionality.

```javascript
// U from CRUD [PUT] ; Update functionality
exports.updateProduct = async(req, res) => {
    try{
        const product = await Product.findByIdAndUpdate(req.params.productId, req.body);    // Get document from the collection
        res.status(200).json({
            msg: "Document Updated Successfully",
            data: product                // "data" is the "product" you will receive from here              
        })

    }catch(err){
        res.status(501).json({
            code: 501,
            msg: "Something went wrong",
            err: err                  // Send error object back
        })
    }
}
```

In the "routes/products.js" file:

```javascript
router.put('/:productId', productController.updateProduct)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688457573444/5932ef5d-7a6a-449e-a4e6-b0fb5e4d6ebd.png align="center")

As you can see it has still returned old data. Does that mean our task failed successfully? The answer is:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688457665188/83300b91-a537-4fa7-bad2-bb2bb8b02e44.png align="center")

Even though it updated that document, it returned old data but in reality, it has updated the content of the document. Just remember, the ID of the object is very crucial while performing these CRUD operations.

Now let us try to implement the last one, "D of CRUD". meaning the DELETE functionality:

```javascript
// D from DELETE [DELETE] ; DELETE functionality
exports.deleteProduct = async(req, res) => {
    try{
        const product = await Product.findByIdAndDelete(req.params.productId);    // Get document from the collection for delete
        res.status(200).json({
            msg: "Document Deleted Successfully",
            data: product                // "data" is the "product" you will receive from here              
        })

    }catch(err){
        res.status(501).json({
            code: 501,
            msg: "Something went wrong",
            err: err                  // Send error object back
        })
    }
}
```

```javascript
router.delete('/:productId', productController.deleteProduct)
```

Suppose now, I want to delete the last entry as in the picture:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688458401648/e07a1ad5-152f-4123-aead-30f1b75f6718.png align="center")

Now when I hit the send button, I will get the following result:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688458461144/aa5da5f5-1412-401c-be6b-7c03ea921eb9.png align="center")

When we will refresh the MongoDB data, we will find the item as deleted:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688458561238/afa512a3-d89e-4a48-a1c5-04f697b4a346.png align="center")

So thus we created CRUD operations using the NodeJS with the Mongoose to connect to the MongoDB. This will be a pivotal concept when we will start to learn MongoDB more in-depth later on.