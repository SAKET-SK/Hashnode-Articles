---
title: "Day 1: NodeJS - Introduction"
datePublished: Mon Mar 27 2023 09:39:53 GMT+0000 (Coordinated Universal Time)
cuid: clfqn1b68001g0ajy27liezw3
slug: day-1-nodejs-introduction
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/UD6KFeA2U7Y/upload/49297b5b05c1fa9d15a0a1ae2de509a4.jpeg
tags: introduction, nodejs, backend

---

NodeJS is a JavaScript Runtime Environment.

Every language i.e Java and Python has its own Runtime Environments. To understand NodeJS, let us understand the concept of Runtime Environments.

### Runtime Environment

Remember the time when Java was released? Why did it become so popular? Because it got identified as Platform Independent Language at that time. Now, what is this "Platform Independent" thing? It means that you need to simply write Java code and it can be executed anywhere, on any system irrespective of the different OS they use.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679749947975/6e67a68c-a387-46f7-8878-b01d6abceaa3.png align="center")

A Runtime Environment helps us to run a program on a particular OS, it might be any OS. Example: JRE (Java Runtime Environment) - It helps to run the same Java code on any OS (Win, Mac, Linux, Solaris)

### Into the Background

JavaScript is an amazing language that offers flexibility; in terms of variables and some other sorts. This kind of flexibility is not available in other languages. But JavaScript had one limitation, it only ran in the browsers.

JavaScript can be run on the console of the browser. Every browser has a part called "JavaScript Engine", which understands the JavaScript code we input in the console. Google Chrome has its JS Engine which has been named V8.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679904374653/8c902cdc-ec1b-43e9-9e48-a32d4f38e5f8.jpeg align="center")

Try typing some JavaScript statements on Command Prompt. Those will be instantly not recognizable. As JavaScript has the most flexibility it had the limitation that it only ran on browsers. So this guy namely Ryan Dahl took the V8 engine code (as V8 engine code is open source) and did some coding on it. Some of the code he did was in C++ as well. After mixing the { V8 + some code }, Node.JS / Node was born (in the year 2009).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679909592723/84c9502c-cd26-4a69-a56b-e39ee092df2b.jpeg align="center")

### So What is Node.JS

Node.JS is an open-source framework. It is mainly a JavaScript Environment. A simple JavaScript file cannot directly run on any machine. If there is a browser, it will run on it, but not anywhere else. So we install Node.Js (will be referring to it as Node sometimes) on the computer. So, Node.Js will work as JavaScript Runtime Environment and will allow the execution of JavaScript code. We will give our JavaScript code to Node and in turn, Node will help us to run the code on our machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679908538743/fd9bf052-81c3-4e7a-aedd-46a2a170b658.png align="center")

This image here explains the importance of Node. We all know that mostly we use HTML, CSS, JavaScript, BootStrap, AngularJs, and ReactJs for Frontend Development. What about Backend? Actually, the Backend of any system is just like a program only. The catch is, it might be written in any language such as Java, .Net, Python, etc. So what is the big deal in it? The thing is, if we want to code our Backend program in Java, we need to learn the entire technology stack comprising J2EE, Spring, SpringBoot, and Hibernate. The same is the case with .Net and Python as seen in above image.

Learning an entire technology stack can be confusing sometimes. So we use Node to code our application's backend. As we progressed through frontend development, it is assumed we have good knowledge of JavaScript which is enough while writing the code for the backend via Node. So, no need to learn big technology stacks. With Node, we can use Express.JS + MongoDB {which deals with data in JSON format}. Owing to these reasons, it is clear that using Node can be the most effective way. Of course, we cannot use Node in case of heavy processing of data. In short, we can use any stack for Backend programming, Node is the easiest solution as we need to learn simply JavaScript. We may use other stacks too, owing to the fact that each stack has its advantages and disadvantages. So we must choose the backend programming stack according to the needs of our application.

**Important Notice:- From what I have learned from this tutorial is that most companies use "ANGULAR + JAVA" which means Angular for Frontend and Java for Backend.**

The Conclusion: NodeJS is an open-source, cross-platform, JavaScript Runtime Environment. We can execute JavaScript code anywhere and on any system. As JavaScript is an awesome language with a good amount of flexibility, using NodeJs for application development is a huge plus.