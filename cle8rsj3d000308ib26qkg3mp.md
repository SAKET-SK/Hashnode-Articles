# Day 8 : Angular - Routing

Ever noticed any website's URL bar with path names? For example, when you are on the login page, ".../login" appears at the end. When you view the profile, ".../profile" appears. The word at the end is known as the specified route, which will tell us what the page will redirect to.

"Routing is **the process of selecting a path for traffic in a network or between or across multiple networks**." We will route i.e redirect to another component when a particular thing is invoked. In Routing, we can simply display any component by writing its name in the browser URL. To perform routing in Angular, we will have to use the **app-routing.module.ts** file.

Inside the **app-routing.module.ts** file,

```typescript
import { DataBindingComponent } from './components/data-binding/data-binding.component';      // Import at the top

const routes: Routes = [
    {path: "data-binding", component: DataBindingComponent}
    // It means when we specify "data-binding", the specified component will appear
]
```

But doing this is not enough. Go to the app.component.html file. Specify &lt;router-outlet&gt; tag. This is where we will get the output of the routers.

```xml
 <router-outlet></router-outlet>
```

Now go to the browser and type:

```markdown
localhost:4200/data-binding
```

Notice what I have typed in front of the URL. The name "data-binding" is specified by us already. When you hit enter, you will see the data binding component will take the place on the main HTML page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676625856659/ed52e3be-0ca8-49ba-a5fd-9d040f73600b.png align="center")

Note that this component I had already created this. Take a look at the URL at the top.

So to recap:  

* Make an entry of the route in the app-routing.module.ts file.
    
* Specify &lt;router-outlet&gt; wherever we want to display the component.
    

---

### Types of Routing:

```markdown
1. Named routing - Handle with a path name e.g "pipe"
2. Default routing - Handle with empty route > ""
3. Redirect routing - Handle by specifying "redirectTo" property
4. Parameterized routing
5. Child routing - Handle by specifying "children" property 
6. Wild card routing - Handle with "**" path
```

**Named Routing:**

In named routing, we specify the name of the route directly, wherever we want to redirect. These all operations will be performed in app=routing.module.ts file. For example:

```typescript
// Do not forget to create and import components at the top.
// Named Routing
  {path: "data-binding", component: DataBindingComponent},
  {path: "directives", component: DirectivesComponent}
```

As you can see, we have specified the names. Whenever we will enter the string in URL bar, we will be able to see the component on our screen.

**Default Routing:**

When we use default routing, it specifies the component which will be loaded first by default. We can achieve Default routing by:

```typescript
// Default Routing -> When we load our app, we will see this component first
  {path: '', component: DirectivesComponent}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676646268190/4256afa2-0906-4663-9857-e328a664250a.png align="center")

As specified, we can see the component which we passed on to default routing, has been loaded first.

**Redirect Routing:**

In Redirect routing, we use the "redirectTo" property. We can use the redirect routing by:

```typescript
 // Re-direction Routing
 {path: "directives", redirectTo:"pipes", pathMatch:"full"},
```

This refers that, when we specify the path as "directives", it will lead us to the Pipe Component. It can be pretty confusing at times, so we will refrain from using it most of the time.

**Parameterized Routing:**

In parameterized routing, we provide a parameter in front to highlight a specific part on the webpage. For example, I have created a table with 7 employees and their id ranges from 1001,.., and 1007.

```typescript
 {path: "pipes/:id", component: PipesComponent}
```

The table is in the pipes component and when the id (the parameter) matched with the one we type, it will indicate its selection.

When we want to use URL-related things, we have to use the "Router" class. For this, we do not have to create the Router object manually. We use Dependency Injection for that. This dependency is specified as the constructor parameter(s) like:

```typescript
 constructor(private router: Router) { }
```

**Child Routing:**

Before proceeding, it is important to specify child routes in the app-routing.module.ts file like:  

```typescript
// Child routing
        { path: "angform", 
            component: AngFormComponent, 
            children:[
                { path:"", component:TdfComponent},
                { path:"tdf", component: TdfComponent},
                { path:"rf", component: RfComponent}
            ]
        }
```

In our case, we have created a component called angular-forms which we will try to cover later. Also tdf and rf (stands for Template Driven Form and Reactive Forms respectively)

It specifies that when we navigate to the forms component, tdf component will be shown by default. We can change the path when we need the rf component.

Wildcard Routing:

Currently in our application, even after we are giving wrong routing, we are not getting the expected output. According to us, it should show page-not-found component. As it should show that component that is not usually seen, we use wildcard routing in that case.

The important thing about this: Always specify this at the end. Inside the routes array, we specify at the end. The reason behind this, is if we specify more routes beneath the declaration of wildcard routing, then those routes are rendered useless as they hold no effect. So be careful remembering this.

```typescript
// Wild Card Routing
  // Sequence Matters!! Always specify this at the end
  {path: "**", component: PageNotFoundComponent}
```

Thus we studied routing and its types.