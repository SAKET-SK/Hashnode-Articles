---
title: "Day 8: NodeJS - Template Engine"
datePublished: Fri Jun 02 2023 18:08:21 GMT+0000 (Coordinated Universal Time)
cuid: clievp9wa000209jt179nfbvz
slug: day-8-nodejs-template-engine
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/6W8H4puOJB0/upload/8d43e1826c2ed8b547173133f48672ed.jpeg
tags: express, nodejs, backend

---

Suppose I got some data to be displayed. I want to populate that data onto the HTML page. So, a template engine is the one which will help us achieve that goal. Similarly, as we do Interpolation in Angular. HTML as a language was not designed to support this concept.

To know more about Template engines:

%[https://expressjs.com/en/guide/using-template-engines.html] 

In this example which we are going to see below, we will be using "hbs" which stands for Handlebars. The "hbs" is Template Engine for Handlebars.

To install hbs, just type...

```javascript
npm i hbs
```

Create a folder named "templates". Inside the templates folder, create one more folder called "views". This is where we will be storing all our views. For direct examples, we will be copying some HTML files and paste them inside them.

Our next step is to obtain a path for this folder where we will be storing our views.

```javascript
const path = require('path')
const viewPath = path.join(__dirname, 'templates/views')
```

Next, we will have to tell ExpressJS which view engine we are using. For that, we need to import hbs first and then we need to configure it.

```javascript
const hbs = require('hbs')
app.set('view engine', 'hbs') // Telling ExpressJS we are using hbs
app.set('views', viewPath)  // This is where the views are
```

Your files inside the view folder should be like this now. Remember they were HTML files first, we changed their extensions to .hbs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685726539744/bdbb3dca-45c5-4ac7-98a6-021de3e4e132.png align="center")

Let us test our code. Here is updated code segment in which we will display the content of index.hbs.

```javascript
app.get('/', (req, res) =>{
    // res.send('ExpressJS works!!!')    // This is the old way
    res.render('index',{               // This is the new way for hbs
        title: 'Home Page',
        author: 'Saket'
    })
})
```

And when we execute our app, we get the following output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685727076645/bf71bc5f-02db-45ca-a4de-822708bdbea6.png align="center")

As you can see, we used hbs view engine to display the home page. We did use the render() function here instead of send(). Take a look at "the index.hbs" file below.

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Express Demo</title>
    <link rel="stylesheet" href="../css/style.css">
</head>
<body style="background-color: cyan">
    <h1>Home Page</h1>
    <h2>This page demonstartes the use of HBS template engine</h2>
    <hr>
    <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Dolor soluta, incidunt dicta distinctio placeat in voluptas excepturi voluptates. Corporis, nobis. Sequi nam dignissimos voluptas odit minima commodi fugiat molestias veritatis saepe, consequuntur in facere, voluptate illo necessitatibus, explicabo deserunt iste molestiae libero laudantium fuga magni! Cupiditate, nemo tempore! Autem, saepe.</p>
    <strong>Title: {{ title }}</strong>
    <strong>Author: {{ author }}</strong>
</body>
</html>
```

Notice the "title" and "author". These were the variable values that we passed. On executing this code now, we get the following output:  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685727571073/3a20b078-f3b0-49ef-ab96-95bc63ecf47b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685727924669/ace039db-740a-4767-926d-381da20d3c09.png align="center")

Thus we can make changes according to us. Now let us create a header and footer for our app. We will keep them in different folders namely "partials". The thing about "helpers" or "partials" is that we need to register them before using them. The partials are the files that we may require at any point unspecified point in our project.

```javascript
const partialPath = path.join(__dirname, 'templates/partials')
hbs.registerPartials(partialPath)
```

Now that the partials are registered, let's take a look at how to use them. I will be making one small change to my "index.hbs" file, by adding this statement. I want my header partial to be present.

```javascript
{{>header}}
```

After we type this thing in our code, we get the output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685728789013/c0c2c50d-3c49-47f7-833b-5747cf747728.png align="center")

Note that, the word "header" is used because our file is named "header.hbs". In case our file name had space in the name or a hyphen, while writing the above syntax, it should be replaced with an underscore.

```plaintext
template.html      -> {{> template}}
template 2.html    -> {{> template_2}}
login view.hbs     -> {{> login_view}}
template-file.html -> {{> template_file}}
```

Thus we learned a thing or two about the template engine "hbs".