---
title: "Day 11: Angular - Services : Creating a Simple Angular Service"
datePublished: Sat Jul 08 2023 16:05:20 GMT+0000 (Coordinated Universal Time)
cuid: clju75qdc00060al01iclatfn
slug: day-11-angular-services-creating-a-simple-angular-service
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/aielvGxZB0g/upload/cfde75b29e5048ab90780712d0bb6f60.jpeg
tags: angularjs, frontend-development

---

A service is a special type of class used in Angular. We can use the service to do many things like sharing data among multiple components, connecting with external resources like data servers, handling data retrieval, manipulation, etc. Service is used by "Dependency Injection". It can be seen that @Injectable decorator can be seen whenever there is a service. This decorator is used to mark the class as an Injectable service.

To create a service, we type the following command:

```plaintext
ng generate service <service-name>
```

In my case, I am creating a folder called Services and creating a service by the name of "Demo". It will be named "Demo.service.ts". It will be called "DemoService" in programs.

```plaintext
ng generate service services/demo
```

It will generate two files yet again, one will be a "spec.ts" file and the other will be a simple "ts" file. The "ts" file is the one we will be writing code in.

we will try to create a simple service, in which we will pass down some data in the JSON format and we will try to write this data on a HTML page using tables. So this is what our "Demo Service" will look like:

```typescript
import { Injectable } from '@angular/core';
@Injectable({
  providedIn: 'root'
})
export class DemoService {
  products = [
    {productId:'P101', productName:'Milk', productPrice:45},
    {productId:'P102', productName:'Bread', productPrice:60},
    {productId:'P103', productName:'Smartphone', productPrice:15000},
    {productId:'P104', productName:'Camera', productPrice:30000}
  ]
  constructor() { }
}
```

This is the sample data we will be writing on the HTML page. Now we need to use this service. Before that, we will create a sample table which will be useful in putting this data.

```xml
<h2>Services Demo</h2>
<table class="table table-dark">
    <thead>
        <tr>
            <th>Product ID</th>
            <th>Product Name</th>
            <th>Product Price</th>
        </tr>
    </thead>
    <tbody>
        <!--WE WILL SEE LATER ON IN THIS BLOG-->
    </tbody>
</table>
```

In the "ts" file of our component, we will create a variable of type "any". This variable will receive all the JSON data.

```typescript
myProducts: any
  constructor(private service: DemoService) {
    // Doing the above line ensures that, in class a property named 'service' will be added
    // and object of 'DemoService' will be created as well as it will be set too.
  }
  ngOnInit(): void {
    this.myProducts = this.service.products
  }
```

This logic is the final nail in the coffin. We are assigning the "myProducts" variable the values which we are passing in JSON format. Now time to write the &lt;tbody&gt;.

```xml
 <tbody>
    <tr *ngFor="let pr of myProducts">
        <td>{{ pr.productId }}</td>
        <td>{{ pr.productName }}</td>
        <td>{{ pr.productPrice }}</td>
    </tr>
</tbody>
```

The names such as "productID", "productName" and "productPrice" should match exactly with the ones we specified in the JSON data. Now if we run our Angular application, we will see this output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688832007243/516f9fdf-1bfa-469e-8e9d-47f9816784af.png align="center")

As you can see, we iterate the whole document by using "\*ngFor" directives. It will spawn the table with exactly the number of entries we pass.

This is just a small example of what a service can do or how to use the service. We can also perform some other complex operations using the services. We will be using a service for a database for performing CRUD applications in our Angular app in later blogs. So that's a basic example of how you can create and use an Angular service. Services provide a way to share data and functionality across components, promoting code reuse and separation of concerns in your application.