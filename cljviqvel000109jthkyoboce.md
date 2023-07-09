---
title: "Day 13: Angular - Guard in Angular and Implementing Login and Logout in Angular App"
datePublished: Sun Jul 09 2023 14:17:28 GMT+0000 (Coordinated Universal Time)
cuid: cljviqvel000109jthkyoboce
slug: day-13-angular-guard-in-angular-and-implementing-login-and-logout-in-angular-app
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/QxOp7pLtzao/upload/d31104f09bb2dcfb7e3aee428269b605.jpeg
tags: json, angularjs, frontend-development

---

In Angular, the Guard is a feature that allows you to control the navigation of the application. It can be used to protect routes based on certain conditions, such as user authentication or authorization. Guards are implemented as classes that can be attached to specific routes to control access to those routes.

To create a guard in Angular, just type this command.

```plaintext
ng generate guard auth
```

Like the components, services, pipes, etc. you can also mention a directory if you want your guard at a specific location.

```plaintext
ng generate guard Guard/auth
```

This will create a Guard named "auth" inside the "Guard" folder. This generates two files:

```plaintext
auth.guard.spec.ts
auth.guard.ts
```

Our main coding will go inside the second one. So let's get started. Our main idea is, unless and until the user is not verified, do not show him all the other application UI. Just show him a login page. Once the user is authenticated via successful login, then automatically grant him the whole access. Now that we have created the guard, let us understand how to use it.

Now see the below image:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688905230692/1e62149d-e73c-4bb3-9699-2b6a77c508c6.png align="center")

We are inside our application. From the inspect panel, select the Application tab. I have already a Guard implemented and I have logged in successfully with the username of "admin1". You can see the value of the "userkey" we are getting; the username. That means it is not empty and the user is logged in. Now refer to this code in the "auth.guard.ts" file:

```typescript
import { Injectable } from '@angular/core';
import { ActivatedRouteSnapshot, CanActivate, RouterStateSnapshot, UrlTree } from '@angular/router';
import { Observable } from 'rxjs';
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
      if(sessionStorage.getItem("userkey") == null){
        return false;   // It means user is yet to log in, thats why the userkey is empty. Thus, show nothing and block access to the route
      }
      else{      // It means user has logged in, store its username and thus grant the access to the route
        return true;
      }
  } 
}
```

As you can see in the above example, our "AuthGuard" is implementing the "CanActivate" interface, in which we are required to write the "canActivate()" method. This method is called when the user navigates to a route protected by this guard.

But how to determine which route is protected by this AuthGuard? We need to register this Guard first. Open the routing module file "app-routing.module.ts" and import the Guard class.

```typescript
import { AuthGuard } from './shared/Guard/auth.guard';  // This should be your directory location where your path is located
```

Now we can use the "canActivate()" property for the desired route to activate the Guard. Now, as we discussed, we will only show the LoginComponent first. Once the user logs in successfully, then show the user the whole app. So, for the LoginComponent, we are using the default routing. For the rest others, we will use Guard.

```typescript
  const routes: Routes = [
  // Default Routing
  {path: '', component: LoginComponent},
  {path: 'login', component: LoginComponent},
  {path: "data-binding", component: DataBindingComponent, canActivate:[AuthGuard]},
  {path: "directives", component: DirectivesComponent, canActivate:[AuthGuard]},
  {path: "pipes", component: PipesComponent, canActivate:[AuthGuard]},
  {path: "pipes/:id", component: PipesComponent, canActivate:[AuthGuard]},
  {path: "comp2", component: Comp2Component, canActivate:[AuthGuard]},
  {path: "mycolor", component: MyColorComponent, canActivate:[AuthGuard]},
  {path: "parent", component: ParentComponent, canActivate:[AuthGuard]},
  // Lazy Loading :- Doing this for only modules becuase modules have huge set of components in them
  {path: "crud", canActivate:[AuthGuard], loadChildren: () => import("./crud/crud.module").then( m => m.CrudModule)},
  {path: "forms", component: AngularFormComponent, canActivate:[AuthGuard],
  children: [
    {path: "", component: TdfComponent},
    {path: "tdf", component: TdfComponent},
    {path: "rf", component: RfComponent}
  ]},
  // Wild Card Routing
  // Sequence Matters!! Always specify this at the end
  {path: "**", component: PageNotFoundComponent}
];
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

As you can see, we have applied the guard to every component except the login and Page Not Found Components. The latter will not be useful when it comes to guard. The former one though is what the user will see on the screen. Thus we have written all the logic regarding the Guard.

---

Now we will try to implement the login and logout functionality. Create a new Component called LoginComponent. Register it in the module file and the routing file as shown above. But before that, we will need to have a group of "Users" who will login and their password. So in our "db.json" file, we will add the Users at the bottom. This is what, our updated "db.json" looks like now.

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
      "salary": 28000,
      "address": "USA"
    },
    {
      "id": 104,
      "name": "Kevin",
      "post": "Test Lead",
      "salary": 32000,
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
  ],
  "Users": [
    {
      "id": "201",
      "username": "admin1",
      "password": "admin@1"
    },
    {
      "id": "307",
      "username": "admin2",
      "password": "admin@2"
    },
    {
      "id": "403",
      "username": "admin3",
      "password": "admin@3"
    }
  ]
}
```

Here we created some usernames and passwords. These will be useful while logging in. Now onto the design.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688909732540/7a98d003-4a77-4645-85b9-6767f81e17d2.png align="center")

Here is what the HTML of our Login page would be:

```xml
<div class="myLoginBox">
    <form>
        <label>Username : </label>
        <input type="text" name="username" placeholder="Enter Username" class="form-control" [(ngModel)]="username">

        <label>Password : </label>
        <input type="password" name="password" placeholder="Enter Password" class="form-control" [(ngModel)]="password">

        <button type="button" class="btn btn-primary mt-2" (click)="login()">Login</button>
    </form>
</div>
```

Let's apply some styling too:

```css
.myLoginBox{
    width: 300px;
    margin: auto;
    border: 2px solid black;
    padding: 15px;
    border-radius: 10px;
}
```

As written in the HTML, once the Login button is clicked, then we are to call the "login()" method, which we will be writing in the "ts" file.

```typescript
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { GlobalService } from 'src/app/shared/services/global.service';
@Component({
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {
  
  username: any;
  password: any;

  userData: any;

  constructor(private service: GlobalService, private router: Router) {}
  ngOnInit(): void {}

  login(){
    // console.log(this.username + " - " + this.password)
    this.service.getRecords("Users").subscribe((res) => {
      this.userData = res;

      const data = this.userData.filter((item:any) => {
        return item.username == this.username && item.password == this.password
      })

      if(data.length > 0){
        this.service.login(this.username);
        this.router.navigate(['/crud']);
      }
      else{
        alert("Username or Password is incorrect");
        this.username = "";
        this.password = "";
      }
    })
  }
}
```

Inside our "global.service.ts" file, we have this chunk of code that we ignored in the previous blog.

```typescript
login(user:any){
    sessionStorage.setItem("userkey",user)
  }
  logout(){
    sessionStorage.removeItem("userkey")
  }
```

Now understand. Our final nails in the coffin have been marked in the "global.service.ts" file, as they set as well as removed the userkey. We are creating variables for username, password, and userData, to store both username and password data from the JSON file.

Then we created another variable called "data". This variable takes the userData such as Username and Password and then it performs a filter operation. This "filter()" method requires a callback function that determines the filtering condition. This callback function will be invoked for each element in the array, and it should return "true" if the element should be included in the filtered array, or "false" if it should be excluded.

The condition for returning the true or false is that if the username entered by the user matches the one in the JSON server AND the password entered by the user matches the one from the JSON server of the corresponding user, then store that in the data variable.

If the length of the data variable is greater than 0, meaning something has been successfully logged. Then by using "this.service.login(this.username)", we are taking that username as a userkey for a session, by using the login() method which was defined by us in a global service. Thus indicating a successful login. Later as the navigation is a success, head to the "/crud" route.

Else, display the message that the username and password are incorrect and clear the filelds for the user to enter them again correctly.

For the logout(), we have created a button for logging out. In the HTML of the navbar, we can see this code:

```xml
<button class="btn btn-primary ms-2" routerLink="data-binding">Data Binding</button>
<button class="btn btn-primary ms-2" routerLink="pipes">Pipes</button>
<button class="btn btn-primary ms-2" routerLink="directives">Directives</button>
<button class="btn btn-primary ms-2" routerLink="parent">Parent</button>
<button class="btn btn-primary ms-2" routerLink="forms">Forms</button>
<button class="btn btn-primary ms-2" routerLink="comp2">Component2</button>
<button class="btn btn-primary ms-2" routerLink="mycolor">My Color</button>
<button class="btn btn-dark ms-2" routerLink="crud">CRUD</button>
<button class="btn btn-danger ms-2" style="float: right"><i class="fa fa-power-off" araia-hidden="true" (click)="logout()"></i></button>
```

In the "ts" file, we write the logic for the logout method:

```typescript
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { GlobalService } from 'src/app/shared/services/global.service';
@Component({
  selector: 'app-nav',
  templateUrl: './nav.component.html',
  styleUrls: ['./nav.component.css']
})
export class NavComponent implements OnInit {
  constructor(private service: GlobalService, private router: Router) {}
  ngOnInit(): void {}

  logout(){
    if(confirm("Are you sure you want to logout 🥹 ?")){
      this.service.logout()
      this.router.navigate(['/'])
    }
  }
}
```

Here we will pop up a confirm dialog box for the user that if he/she wants to log off. If he says yes, that means it returns true then, call the "logout()" from the global service which effectively removes the userkey from the session, thus logging you out and then navigating automatically to the default page, which means in our case the login page.

---

Thus, in this way, we have created a fully functional Angular app, which has login functionality. It takes data from the JSON file we created. If the login credentials match, the app works well. In case of an error, it throws an error. Every main concept of Angular has been implemented throughout the creation of this app. Please refer to the blogs in the Angular series for detailed codes. You may also visit my GitHub repository where I have all the web development-related source code. You can visit the GitHub Repo by clicking [here](https://github.com/SAKET-SK/FULL-STACK-WEBDEV-REPO).