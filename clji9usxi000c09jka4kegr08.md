---
title: "Day 11: NodeJS - All about MongoDB Atlas"
datePublished: Fri Jun 30 2023 07:47:35 GMT+0000 (Coordinated Universal Time)
cuid: clji9usxi000c09jka4kegr08
slug: day-11-nodejs-all-about-mongodb-atlas
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cijiWIwsMB8/upload/941839cc9388f1e0b01a4421628d235f.jpeg
tags: express, mongodb, nodejs

---

MongoDB Atlas is a service provided by MongoDB which gives us a cloud-based database. Our database will be in the cloud which effectively means the MongoDB Server.

You may be wondering why there MongoDB Atlas is referred to here. The thing is, whatever data we are going to generate or use while performing NodeJS operations, we are going to store that data in this cloud-based database.

Let us start using MongoDB Atlas. Search for MongoDB Atlas and sign up yourself with Google ID. Once you log in after registering yourself, you will see the following screen.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687786828422/7877fbcb-1cdc-4e38-b80c-4c7d5e15ea56.png align="center")

To view your "Clusters", click on "All Clusters" on the navigation bar on top.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687786982071/7099939b-f7f3-4298-8796-0875d4a02972.png align="center")

A Cluster is basically a collection of databases, which we try to run on a particular instance. Let us try to build a database here. Click on the **"Build a Database"** button.

After clicking on that button, we will see the following screen:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688100914968/93359b7d-13af-44f5-8310-78d0f5a8b684.png align="center")

We will be needing to select the FREE option, as we are using the database for our learning purpose. After selecting it, click the **Create** button below.

Once you click the create button, it will list out some security provisions you will need to provide. It will ask for Username, Password for a user to be added to use the database, from where you want to connect, the Local environment or Cloud Environment, and your IP address.

Input any username and password you like for the user. Later select the Local environment as an option to connect from. Later while adding the IP address, click on the **"Add my IP Address"** button. This will add an entry of your machine's IP address that you can use.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688101264434/0cc33a01-7695-4b80-9cd7-0f76084024b9.png align="center")

As you can see, our machine's local IP Address has been added. But we will do a small change. We will add another entry as shown below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688101402094/a7e00de4-00ab-45eb-a356-e3dba48c9aa0.png align="center")

This means universal. The purpose of the first IP Address is to give access to the database to the machine if the IP Address matches. Just because you have a username or password does not finish things here. By adding the second entry as shown above, you are telling me that this database can be accessed on ANY IP ADDRESS after I provide my username and password. The reason we do this is that the first entry is a dynamic IP address. It will change if I restart the machine. And thus we will start getting errors about being unable to connect to the database. In order to avoid this issue in the near future, we are doing this part.

Click on "Finish and Close" once this is all done. You will see the following screen.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688101895617/e7125764-a0c4-4474-9323-f1b40c6664ad.png align="center")

Click on the "Connect" button as in the above image. Once you click it, you will see the following popup.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688108267843/4f0396dd-af67-4157-9563-50cb6e2129e5.png align="center")

You may select the option you think suits you. I am selecting the "Compass". MongoDB Compass is a GUI application. By using this application, we will connect our cloud database to the app which we wrote. We will be easily able to create and work with databases with MongoDB Compass. Once you select the Compass option, the following popup appears:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688108581713/805d1139-0a0a-46ab-935e-888def404fe9.png align="center")

If you have MongoDB Compass, then select the option accordingly. If not, it will tell you to choose your operating system and after clicking the download button, you will be able to install it.

Secondly, the step-2 URL is the link that we will be using in the MongoDB Compass, to establish the connection with our database. Same URL we will be using in our JavaScript code too. We have written all the code, but we have to store that data in the database too.

Once it is installed, you will be able to run the MongoDB Compass inside your machine. It will look something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688109036625/92a154c7-da53-4878-9243-156960812c67.png align="center")

Notice the "URI" (Uniform Resource Identifier) localhost:27017. The reason we can need this is that if we were to use MongoDB on our OWN machine. At this step, we have only installed MongoDB Compass, which is a GUI tool that helps us to establish the connection to MongoDB Database.

Now we will copy the connection string from our previous [image](https://cdn.hashnode.com/res/hashnode/image/upload/v1688108581713/805d1139-0a0a-46ab-935e-888def404fe9.png) and only replace the &lt;password&gt; with the password that we set. After that click on the "Connect" button. Once the connection has been established, we will see the following window.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688109590516/ba211397-548a-45c0-82a1-a16dc2d02eb2.png align="center")

Lets us create a Database here. To do so, click on "+" in the Left Menu in front of the Database. It will give us this window:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688109948794/35b78d1d-1828-4316-a6e7-11d983d28ed4.png align="center")

Simply hit the "Create Database" button. You will be able to see this view.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688110019228/0e80fba7-4b7f-4f2c-b796-7176798b71b8.png align="center")

Here on the left-hand side, you can see the database name and students as the Collection. A Collection is similar to a Table. Similarly, the term Documents is similar to Records in a Table.

Once you are ready to add the data, click on the Add data button at the top.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688110576310/5728bdab-b486-4c24-9366-e6dd781a5524.png align="center")

We will be inserting it in the form of JSON. So I am clicking on "Insert Document". Let us insert data now. You can refer the sample change I did below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688110751429/85e3915f-262e-428a-b63f-61b20c9075e2.png align="center")

Click on Insert. You will see the following change on the main page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688110818652/52667453-943e-42c9-ba40-aeae5ec23cf2.png align="center")

To check if the collection is there, Go to MongoDB Atlas on the Web browser. Click on Cluster0 and then click on Collections navigation. You will see the data as below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688110996254/a2376e96-18d2-49b8-9fee-80af5047349a.png align="center")

Notice that it has created its own ID as MongoDB works highly with ID and Objects. The purpose of doing so is to make it unique across all the objects.

Thus we can see our created database via MongoDB Compass can be seen on the Web via MongoDB Atlas. We will see the database operations later on in next articles.