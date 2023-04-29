---
title: "Day 2: NodeJS - Node REPL and Modules"
datePublished: Sat Apr 29 2023 07:59:37 GMT+0000 (Coordinated Universal Time)
cuid: clh1ozglb000e09l03n937scm
slug: day-2-nodejs-node-repl-and-modules
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/HsTnjCVQ798/upload/6576162807b863d7dde2ff570abfbc8b.jpeg
tags: nodejs, modules, frontend-development, backend-development

---

### Node REPL

REPL Stands for (Read-Eval-Print-Loop). It is a command line-based tool in which we can issue Node commands or JavaScript code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680079832462/91d7bb1f-d8dd-430b-9a2d-63a96b1314d4.jpeg align="center")

We can perform a variety of operations in this REPL tool. In Image 1 which is the one on the top, we can see that we can execute JavaScript code without any hassle and the fun part is, it runs anywhere. To enable REPL in VS-Code, just type "node" and hit enter.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680080040364/04059a98-edc1-4c12-a82b-b3af24e3fe6e.jpeg align="center")

We can also perform operations like saving our command line code in some files as well as loading a JavaScript file for execution in REPL.

```markdown
.save <name_of_file>.js   // For saving the REPL Command line code in JS File
.load <name_of_file>.js   // loading file in REPL
```

When you finished your work with REPL, Press "Ctrl + D" or ".exit" to come out of it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680080793680/4639a87b-d2f4-42d9-a63e-e9d46634a3f5.jpeg align="center")

We can also load the JavaScript code file directly by specifying its path.

### Modules

There are 3 types of Node Modules:

* Core Modules (Provided by default by Node.Js)
    
* Local Modules (will be developed by us)
    
* Third-Party Modules (Downloaded from sites such as NPM)
    

Below are some of the examples and uses of Core Modules of Node.JS

```plaintext
    http           -> To create http server in node.js
    os             -> Provides info about operating system
    path           -> To handle the file path
    querystring    -> To handle url query string
    fs             -> To handle file system in the computer 
    util           -> To access utility functions
    url            -> To parse the url strings
```

### Global Objects

In NodeJS, there are some special objects which we can access anywhere in our program code. They are known as Global objects. We can trigger the Global Objects as per our needs. Let's have a look at an example.

Suppose I want to obtain the file name and directory name of the file for which I am currently writing my JavaScript code. I will simply execute below code snippet:

```javascript
console.log(__dirname)
console.log(__filename)
```

When I tried to execute this, I got the following output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682745757949/33eb3cfe-3fd0-4543-aed1-822d613d66b3.jpeg align="center")

The first line gave the name of the directory we are currently in i.e. the directory from which we fired this Global Object. The second line gave us the name of the file from which the Global Object was written. In my case, I have named the file as "core-modules.js"

### Path Module

Path module is usually used when we work with folder structures i.e. directories and files. When working with directories, a path module is very useful. To use Path Module we need to write the following line of code at the start.

```javascript
const path = require('path')
```

The reason we didn't use any '.' or '/' in the location of the 'path module' is that it is a core module. It is always present in our program, we just need to import it.

Let us look at some of the methods provided by the path module.

```javascript
// 1) basename -> gives the name of the main folder in which our program is
console.log(path.basename(__dirname))
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682747363837/65e1965d-2f99-4663-b4da-6281e3b446f6.jpeg align="center")

```javascript
// 2) dirname -> It will return the path of the folder in which we are currenlty. Have a look for reference of the path in below image
console.log(path.dirname(__dirname))

// 3) extname -> returns extension of the current file
console.log(path.extname(__filename))
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682747549110/987df7b4-cdda-4434-bf2c-b0c35adb0830.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682747696110/0bf3b990-dc61-4dc2-aa2f-f06ada836584.jpeg align="center")

### OS Module

Os module is helpful when we are working on operating systems. We might run our programs on Windows, Mac, Linux, etc. By using Os Module, it may be able to provide some information about the operating system that the developer is currently using. To use it, we need to write this code statement at the start of our program:

```javascript
const os = require('os')
```

Let's have a look at some of the methods provided by the Os Module:

```javascript
// 1) arch -> returns the architecture of OS we are currently using
console.log(os.arch())
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682748712600/b85404cc-bb76-4448-9557-3a57b6555da9.jpeg align="center")

```javascript
// 2) homedir -> returns the Home Directory on the System we are using
console.log(os.homedir())
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682748837591/807cb615-4222-4b1d-82af-3b541391c29a.jpeg align="center")

```javascript
// 3) platform -> returns the name of OS platform name which we are currenly using 
console.log(os.platform())
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682749088491/322406ec-843d-4772-8e21-5a5ccabd7051.jpeg align="center")

### Third-Party Modules:

A Module that is installed via NPM (Node Package Manager) and is available online is known as Third Party Module.

EXAMPLE 1: - I want to import the "[chalk-animation](https://www.npmjs.com/package/chalk-animation)" package which is available online and can be installed via NPM. When the package is installed we can use it in our web application for different styles and purposes. To install the above Third-Party Module, we will fire up this command on our terminal:

```plaintext
npm i chalk-animation
```

We will require some JavaScript code now:

```javascript
import chalkAnimation from 'chalk-animation';
chalkAnimation.rainbow('Welcome to Node.js Third Party modules...');
```

```json
{
  "type": "module",
  "dependencies": {
    "chalk-animation": "^2.0.3"
  }
}
```

This is how your "package.json" should look after the installation of the above Third-Party Module. Now here is what we get as output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682754466633/b1fe695c-b92f-46b9-a11f-0114fd09d91a.jpeg align="center")

EXAMPLE 2: - There is another Third-Party Module namely "[validator](https://www.npmjs.com/package/validator)" which validates everything from Email, Numbers, Strings, etc. To install it we are going to fire the following command:

```plaintext
npm i validator
```

```javascript
import validator from "validator";
console.log(validator.isEmail("saketk@gmail.com"))
console.log(validator.isEmail("saketk#gmail.com"))
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682754978032/7ee0cd52-2d13-4eed-ad38-33203ee052b5.jpeg align="center")

Have a careful look at how it validates on the basis of signs. Similarly, there are tons of exciting Third-Party Modules which are available on the site of npm which are easy to install and ready to use.

Therefore we learned today basics of NODE REPL to different types of Modules which we can use in our web applications.