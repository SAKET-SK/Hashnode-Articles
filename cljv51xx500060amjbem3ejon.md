---
title: "Day 12: Angular - CRUD operations using JSON Server"
datePublished: Sun Jul 09 2023 07:54:10 GMT+0000 (Coordinated Universal Time)
cuid: cljv51xx500060amjbem3ejon
slug: day-12-angular-crud-operations-using-json-server
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/v89zhr0iBFY/upload/f2d8bc539ad46f3c97b23e0e8dd59570.jpeg
tags: angularjs, frontend-development, crud

---

Any app is incomplete without a CRUD functionality into it. When we implement all CRUD operations it feels like a complete app. In this blog, we will see how to create a separate module in which we will handle and manage CRUD operations using the JSON server.

So, what's a Module? It is a collection of single or multiple files which are used to perform a specific task. The biggest advantage of using the modules is reusability. The creation of modules helps in reusing them anywhere in other projects too.

> In Angular, the root module is "app.module.ts"

To create a module, type this command in the terminal:

```plaintext
ng generate module <module-name>    // creates the module without routing
ng generate module <module-name> --routing   // creates the module with routing
```

We will create a module with routing, as we are going to need it in the future. In my case, I am naming my module "crud".

We need to implement a way to get data from a Server. We will use a dummy server for the same. To do so, we need to install a JSON server, from where we will use some data to perform our operations.

To install the JSON server, use this command:

```plaintext
npm install -g json-server
```

Next, we need to create a dummy file named "db.json". In this file, I will now post the random data which I want to work upon. Refer this below structure of my "db.json" file.

```json
{
  "Employees": [
    {
      "id": 102,
      "name": "Alex",
      "post": "Junior Manager ",
      "salary": "20000",
      "address": "Moscow"
    },
    {
      "id": 103,
      "name": "Joe",
      "post": "Associate",
      "salary": "28000",
      "address": "USA"
    },
    {
      "id": 104,
      "name": "Kevin",
      "post": "Test Lead",
      "salary": "32000",
      "address": "Berlin"
    },
    {
      "name": "Albert",
      "post": "QA Lead",
      "salary": "50000",
      "address": "Brazil",
      "id": 105
    },
    {
      "name": "Alex",
      "post": "Intern",
      "salary": "5000",
      "address": "USA",
      "id": 106
    },
    {
      "name": "Peter",
      "post": "Trainee",
      "salary": "11000",
      "address": "Rome",
      "id": 107
    }
  ]
}
```

We will be working on Employee data for the demonstration of CRUD applications. Now let us create components namely "DashboardComponent", "AddComponent" and "EditComponent". Make sure you navigate inside this "crud" module in the terminal and type these commands:

```plaintext
.../crud> ng g c add
.../crud> ng g c dashboard
.../crud> ng g c edit
```

We require to add and edit as separate components as we will have to navigate to a different page for that. For reading, no such component is needed and we can implement delete functionality on our own. But before that, you will need to register your components. So the contents of your "crud-routing.module.ts" file and "crud.module.ts" should be as follows:

```typescript
// crud-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AddComponent } from './add/add.component';
import { DashboardComponent } from './dashboard/dashboard.component';
import { EditComponent } from './edit/edit.component';

const routes: Routes = [
  {path:'',component: DashboardComponent},    // By default show this
  {path:'dashboard',component: DashboardComponent},
  {path:'add',component: AddComponent},
  {path:'edit/:id',component: EditComponent}  // we need id on which component to edit
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class CrudRoutingModule { }
```

```typescript
// crud.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { CrudRoutingModule } from './crud-routing.module';
import { DashboardComponent } from './dashboard/dashboard.component';
import { AddComponent } from './add/add.component';
import { EditComponent } from './edit/edit.component';
import { FormsModule } from '@angular/forms';

import { Ng2SearchPipeModule } from 'ng2-search-filter';   // We will be implementing pagination for good effect
import { NgxPaginationModule } from 'ngx-pagination';
@NgModule({
  declarations: [
    DashboardComponent,
    AddComponent,
    EditComponent
  ],
  imports: [
    CommonModule,
    CrudRoutingModule,
    FormsModule,
    Ng2SearchPipeModule,
    NgxPaginationModule
  ]
})
export class CrudModule { }
```

Now we are all set with the components registered and we can now use them in our functionality. We will be creating our final product like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688879306785/17d0f5a7-8ddc-4ff3-8255-8acca655b2a9.png align="center")

The functionality of Logout has been included as we will add the Login function later on. So do not bother about Logout right now. The "All Concepts" nav bar will continue to appear as we have included a service for that. We will see that later. For now, focus only on CRUD. Now, let us create a dashboard first.

---

DASHBOARD:

```xml
<div class="row">
    <div class="col-sm-8">
        <button type="button" class="btn btn-primary mb-2" routerLink="add"><i class ="fa fa-plus-square" aria-hidden="true"></i></button>
    </div>   <!--This is add button, Clicking on it will lead to add component, where the logic is written-->
    <div class="col-sm-4">
        <input type="text" name="search" id="" class="form-control" placeholder="Searching Someone..?" [(ngModel)]="term">
    </div>  <!--This is a search box. We can search anyone-->
</div>
<table class="table table-light">
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Post</th>
            <th>Salary</th>
            <th>Address</th>
            <th>Action</th>
        </tr>
    </thead>
    <tbody>
        <tr *ngFor="let e of empData | paginate: { itemsPerPage: count, currentPage: p } | filter:term">
        <!--Doing multiple things here, for loop to obtain all data, doing pagination and using filter for search bar-->
            <td>{{ e.id }}</td>
            <td>{{ e.name }}</td>
            <td>{{ e.post }}</td>
            <td>{{ e.salary }}</td>
            <td>{{ e.address }}</td>
            <td>
                <button type="button" class="btn btn-outline-info" [routerLink]="['edit',e.id]"><i class ="fa fa-pencil-square-o" aria-hidden="true"></i></button> <!--Tis will take us to edit component, where edit logic is written-->
                💠
                <button type="button" class="btn btn-outline-danger" (click)="delete(e.id)"><i class="fa fa-trash-o" aria-hidden="true"></i></button> <!--Calling delete function to remove the record-->
            </td>
        </tr>
    </tbody>
</table>
<div class="text-right">
    <pagination-controls (pageChange)="p = $event"></pagination-controls>   <!--for pagination-->
</div>
```

For pagination, make sure you install the pagination module from the npm site. Now let us look at the "ts" file.

```typescript
import { Component, OnInit } from '@angular/core';
import { DbService } from 'src/app/shared/services/db.service';
import { GlobalService } from 'src/app/shared/services/global.service';
@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.css']
})
export class DashboardComponent implements OnInit {
  constructor(private globalService: GlobalService) {}
  empData: any    // To store our employee data
  term: any     // for search bar

  p: any = 1;   // these both for pagination, count represents number of rows per page
  count: any = 5;

  ngOnInit(): void {
    this.globalService.getRecords("Employees").subscribe((res) =>
      // console.log(res)
      this.empData = res   // Get all records on screen
    )
  }

  delete(id:any){
    this.globalService.deleteRecord("Employees",id).subscribe(() => (
      alert("Record Deleted Successfully")
    ))
  }
}
```

You saw a glimpse of "global service". We will use this global service to perform literally all the operations we can inside this CRUD app.

To use this "global service", create it first. Later use the "HTTP client" as we will be obtaining data from the JSON server. This will be our global service code:

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class GlobalService {

  baseURL:string = "http://localhost:3000"   // where our data is stored in JSON server
  constructor(private http: HttpClient) { }   // Injected HTTP Client

  getRecords(table: string){       // READ records
    const url = `${this.baseURL}/${table}`;
    return this.http.get(url);
  }

  addRecord(table:any, empData: any){   // CREATE records
    const url = `${this.baseURL}/${table}`;
    return this.http.post(url,empData)
  }

  getRecord(table:any, id:any){      // READ a SINGLE record
    const url = `${this.baseURL}/${table}/${id}`
    return this.http.get(url)
  }

  updateRecord(table:any, empData:any){   // UPDATE a record
    const url = `${this.baseURL}/${table}/${empData.id}`
    return this.http.put(url, empData)
  }

  deleteRecord(table:any, id:any){    // DELETE a record
    const url = `${this.baseURL}/${table}/${id}`;
    return this.http.delete(url)
  }

// We will see this later; for now ignore this
  login(user:any){
    sessionStorage.setItem("userkey",user)
  }
  logout(){
    sessionStorage.removeItem("userkey")
  }
}
```

The "table" refers to "Employees" JSON data which we have already. These methods are the basic implementation of CRUD operations. Let us focus one by one on CRUD.

---

CREATE OPERATION (C of CRUD):

The Create operation refers to adding a new record inside our app. We will write the logic of adding an employee record so that it will be displayed on the Dashboard. To trigger Create operation on Dashboard, we have created a button as shown in the previous screenshot. Clicking it will redirect us to "AddComponent" thanks to routing. Now let us see the logic of it.

```xml
<form #myForm="ngForm" (submit)="addData(myForm.value)">
    <label>Enter Employee Name : </label>
    <input type="text" name="fname" class="form-control" ngModel required>

    <label>Enter Employee Post : </label>
    <input type="text" name="epost" class="form-control" ngModel required>

    <label>Enter Employee Salary : </label>
    <input type="text" name="salary" class="form-control" ngModel required>

    <label>Enter Employee City : </label>
    <input type="text" name="ecity" class="form-control" ngModel required>

    <button type="submit" class="btn btn-success mt-2" [disabled]="myForm.invalid">Save Details</button>
</form>
```

We are basically creating a form named "myForm" in which we will input all the data and take it inside via the submit button. Whenever the (submit) gets triggered, it will call the "addData(myForm.value)" function. The button will be disabled until all data fields are not empty.

Now we need to write the "addData" function inside out the "ts" file.

```typescript
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { GlobalService } from 'src/app/shared/services/global.service';
@Component({
  selector: 'app-add',
  templateUrl: './add.component.html',
  styleUrls: ['./add.component.css']
})
export class AddComponent implements OnInit {
  ngOnInit(): void {}
  constructor(private service : GlobalService, private router: Router) {}

  addData(data:any){
    const empObj = {
      name: data.fname,
      post: data.epost,
      salary: data.salary,
      address: data.ecity
    }
    
    this.service.addRecord("Employees",empObj).subscribe( () => {
      alert("Record Added")
      this.router.navigate(['/crud'])  // After successfull insertion of data, go back to home page automatically
    })
  }
}
```

Now understand the flow. In this TypeScript file, we are injecting the Global Service and the Router. Later we create an object in which we will store all the data which the user enters. Make sure the names match the ones mentioned in the HTML file.

Now using our Global Service method "addRecord()".

```typescript
addRecord(table:any, empData: any){
    const url = `${this.baseURL}/${table}`;
    return this.http.post(url,empData)
  }
```

Here we are passing the table; in other words, all the Employee data we have on the JSON server and an object. Now as we look into the earlier typescript file, when we call this data, we pass the name of the table, which is Employees, and the object in which we stored all the data user entered. Now the only thing that remains is putting that data on that JSON server URL so that it might get fetched and we may see a result. So, we are using the post() method of HTTP to signify where to put that data (URL) and which data to put (empData object).

Now for the demo. We are adding the following new data to our server. After entering this, I will press the "Save Details" button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688886168606/0dadca83-129f-4e3e-93cc-c42b02de9aa6.png align="center")

After that, it will give an alert of the Record added and we can see the newly added record.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688886284414/2f230e4e-04c2-49ac-9c55-2a1fae66245b.png align="center")

Thus our Create operation has been done successfully.

---

READ OPERATION (R of CRUD):

The way we can see this data is referred to as Read operation. To obtain all the records, we have used the following method:

```typescript
getRecords(table: string){
    const url = `${this.baseURL}/${table}`;
    return this.http.get(url);
  }
```

Here we simply write this method above all and call the get() method of HTTP. By doing so, we can get all the records on our dashboard.

But, what if I wanted to Read only a specific record? In this case, we will require the ID of the record we need to write this method:

```typescript
getRecord(table:any, id:any){
    const url = `${this.baseURL}/${table}/${id}`
    return this.http.get(url)
  }
```

Usually, we don't do so (Searching for one specific record manually) as we will be having search functionality later on. But this functionality will be useful when we are editing a specific record.

---

UPDATE OPERATION (U of CRUD)

We will now implement the function to edit the records to make it a more powerful app. Remember the new record we inserted in Create section, we are going to modify that. For the Update function, the ID of the record is required. Here is the update function as in Global Service:

```typescript
updateRecord(table:any, empData:any){
    const url = `${this.baseURL}/${table}/${empData.id}`
    return this.http.put(url, empData)
  }
```

This method will be triggered when we click on the Edit button in our Action Column from our application. The HTML of this EDIT is no different than Add Employee but with minor word changes.

```xml
<form #myForm="ngForm" (submit)="putData(myForm.value)">
    <label>Enter Employee Name : </label>
    <input type="text" name="fname" class="form-control" [ngModel]="empData?.name" required>

    <label>Enter Employee Post : </label>
    <input type="text" name="post" class="form-control" [ngModel]="empData?.post" required>

    <label>Enter Employee Salary : </label>
    <input type="text" name="salary" class="form-control" [ngModel]="empData?.salary" required>

    <label>Enter Employee City : </label>
    <input type="text" name="address" class="form-control" [ngModel]="empData?.address" required>

    <button type="submit" class="btn btn-warning mt-2" [disabled]="myForm.invalid">Update Details</button>
</form>
```

In the "ts" file, we need to add the "putData(myForm.value)" function.

```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Router } from '@angular/router';
// ActivatedRoute is used here, for retriving the original data and perform operations on it.
import { GlobalService } from 'src/app/shared/services/global.service';
@Component({
  selector: 'app-edit',
  templateUrl: './edit.component.html',
  styleUrls: ['./edit.component.css']
})
export class EditComponent implements OnInit {
  id: any
  empData: any

  constructor(private route: ActivatedRoute, private service: GlobalService, private router: Router) {}

  ngOnInit(): void {
    this.route.paramMap.subscribe((para) => {
      this.id = para.get('id')     // Obtain the ID and retrieve the record for updation
      this.service.getRecord("Employees",this.id).subscribe((res) => {
        this.empData = {...res}    // Doing this will store the data in object format
      })
    })
  }

  putData(val:any){
    const emp = {
      id: this.id,   // SAME ID
      name: val.fname,   // OLD VALUES
      post: val.post,
      salary: val.salary,
      address: val.address
    }

    this.service.updateRecord("Employees",emp).subscribe(()=>{
      alert("Record Updated")
      this.router.navigate(['/crud'])
    })
  }
}
```

The significance of obtaining previous values is that we will be able to preload the old data, only for us to type new data more effectively.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688888351100/908863e0-02a3-4184-86e0-5f93312ecf70.png align="center")

These all fields were already filled on when we click on the Edit button. Here we can modify the data and click the update button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688888547150/166e7258-b7c3-4e88-9808-fae119d26def.png align="center")

Now after clicking the Update button, we can notice that our data has been updated.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688888622748/f94fc6c1-6783-4d5a-9e77-d9401814a865.png align="center")

---

DELETE OPERATION (D of CRUD)

We have implemented the button for Delete in Dashboard Component itself.

```xml
<button type="button" class="btn btn-outline-danger" (click)="delete(e.id)"><i class="fa fa-trash-o" aria-hidden="true"></i></button>
```

Now we need to write the delete() method in the "ts" file.

```typescript
delete(id:any){
    this.globalService.deleteRecord("Employees",id).subscribe(() => (
      alert("Record Deleted Successfully")
    ))
  }
```

There is also a method in Global Service of which the above method uses. We need to pass the ID of the record we are deleting:

```typescript
deleteRecord(table:any, id:any){
    const url = `${this.baseURL}/${table}/${id}`;
    return this.http.delete(url)
  }
```

This method will be triggered when we hit the Delete button. Now I want to delete the latest record we updated. So I will hit the delete button in front of it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688889094428/cd841424-8836-49c6-b0c9-7bab2c2c55f0.png align="center")

As you can see, the record is gone.

---

Thus we have seen all about the CRUD operations in our application. In our later blogs, we will try to make our application more secure by adding Admins as well as the Login and Logout Functionalities too. Stay tuned.