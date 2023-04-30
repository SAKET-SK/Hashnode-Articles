---
title: "Day 3: NodeJS - NodeJS Architecture & NodeJS Events"
datePublished: Sun Apr 30 2023 08:47:38 GMT+0000 (Coordinated Universal Time)
cuid: clh3652mi000f0al496kdfhwo
slug: day-3-nodejs-nodejs-architecture-nodejs-events
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/-HIiNFXcbtQ/upload/e688e368ca2655638c48e01b33b1eae5.jpeg
tags: nodejs, backend

---

Today we are going to learn about the architecture of NodeJS.

NodeJS is based on the V8 engine which is in turn a JavaScript Engine. Assuming that most of the part of the code of NodeJS is in JavaScript and the rest of the code is in C / C++. The reason being the V8 engine is partially coded in C++.

One thing worth noticing is that JavaScript does not support the concepts of threads. Threads are usually responsible for handling multiple processes at the same time in an application. JavaScript does not have this concept, nor does NodeJS as it is mostly coded in JavaScript. JavaScript uses a concept called "Event Loop" instead.

We can now look into Blocking and Non-Blocking applications. In a Blocking application, the process waits. It will first complete the tasks at hand and only after finishing it, move on to the next task. If this Blocking will happen, then you might be able to imagine how tedious the task would be. Thus in Blocking, things will happen one after the other, kind of how synchronous (One at a time, others will wait) type of applications work.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682766333104/bfe98cc9-fe31-48bb-8c30-808eb34c3567.png align="center")

As seen in the above image, Blocking is quite tedious. So there is also a term called Non-Blocking (Asynchronous). It implies that it will handle multiple things running simultaneously.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682767544457/9e14a718-cdd3-4c86-8c74-46ef75bfc5ea.png align="center")

But as mentioned earlier, JavaScript does not support Threads. Then how come this Asynchronous execution is happening?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682767135060/11e0d2b9-eeb6-405a-aa78-84089333009b.jpeg align="center")

One thing we should know is that NodeJS is based on the JavaScript engine V8, which is written in JavaScript & C++. JavaScript as a language does not support multithreading but C++ does. This is how NodeJS, despite being said that it is based on JavaScript and also JavaScript does not support multithreading, runs multiple processes as it obtains the support via C++. Here is where "Event Loop" comes into the picture.

Let's understand the concept of Event Loop with the help of an example.

Consider a Restaurant. There are numerous customers and numerous chefs working together to run it. Take a look at the reference diagram.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682779959785/ccdc305d-efa2-47fc-8911-c30cbaaaba59.jpeg align="center")

The Single Thread is the single waiter who is taking orders from customers and assigning them to chefs. The waiter took the order from Customer C1 and assigned it to Chef\_1. At the same time, Customer C2 arrives and the waiter takes the order and assigns it to Chef\_2. Now suppose at this moment, Chef\_1 completes his order and his order is dispatched thus implying that he is free now.

Dispatching the order indicates the Callback stating that the event has been completed and we can free the particular resource.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682780318625/f10c18d1-1350-4278-bfba-49698cfa54aa.png align="center")

The Event Queue is like customers. The Thread Pool is basically the chefs preparing the order i.e. executing the event. The key thing is that during this Event Loop, it will never Block, which means no event will be stalled. The waiter i.e. an Event Loop does not have a big role, his job is to manage everything.

NodeJS is single-threaded only, even though it performs multithreading sometimes. The reason is that multithreading is handled internally (C++ Library). So, in conclusion, we can say that the NodeJS Architecture runs "Single Threaded - Non-Blocking", Asynchronous programming which is way may memory efficient. All this is achieved via Event Loop.

### Events

Events are like some occurring function inside. Any action that happens is an Event. Part of the fact is that NodeJS is also known as Event Driven, as most of the functioning happens via Events i.e. Event gets emitted, someone gets assigned to complete that event and thus the event is finished.

Now let's try to create an event.

```javascript
const event = require('events')   // 1) core module
const emitter = new event.EventEmitter() // 2) Emitter For emitting the event

emitter.on("party", function(){   // listener function
    console.log("Party started....") // 4) The output when "party" is emitted
})

emitter.emit("party")     // 3) The name to be invoked
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682839995930/e67549f0-99f9-42bb-be0a-f23731d56240.jpeg align="center")

In our above example, we emit the event "Party". From lines 4-6, we are specifying what action will occur once "Party" is emitted. So we specify a listener function that will do the necessary thing required.

Thus we saw in detail what NodeJS Architecture is as well as a bit more about the events.