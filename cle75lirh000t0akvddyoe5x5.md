# Day 5 : Angular - Attribute Directives

In the previous article, we have seen Structural Directives (ngIf, ngFor, ngSwitchCase). In this article, we will learn more about Attribute Directives.

We will be covering the following

Attribute Directives:

* ngStyle
    
* ngClass
    
* ngModel -&gt; Used when there is the use of two-way binding
    

### ngStyle:

Using this directive we can use CSS properties using the Component property. For its usage, you will have to declare a property in a component file (TS file) and assign it to the ngStyle directive for any element (HTML file). Let's look to this in the below example:

```typescript
// Condition for setting the style
myCondition = true
// Creating a new property 
myNewStyle = {
    color: this.myCondition ? "green" : "blue",
    fontSize: "50px"
}
```

We have created our custom property in which we are specifying that if the condition is true, then the color of the text is green or else it should be red. Also, the font size of the text will be 50px.

Now let us assign it in an HTML file.

```xml
<div [ngStyle] = "myNewStyle">This is the paragraph we will use for demonstration of ngStyle</div>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676553054770/ed85f691-6344-45c9-9cd9-a3c61b2a7f2e.png align="center")

As you can see, the custom-style property created by us has been assigned to that particular element.

### ngClass:

This is very similar to ngStyle we saw but with a change. We can apply CSS classes that are part of our .css file. Its usage is quite similar to ngStyle too. Let us look by referring to the below example:

```css
.txtDanger{
    color: red;
    font-family: cursive;
    background-color: aqua;
}
```

```typescript
flag = true    // Just for setting condition
  myNewClass = {
    "txtDanger": this.flag
    }
```

```xml
<div [ngClass] = "myNewClass">This is the paragraph we will use for demonstration of ngClass</div>
```

We will obtain the following output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676554490304/c75768db-e0bc-4bfe-bcb0-673d37a5f7fd.png align="center")

As you can see the properties we mentioned in the .css file has been mentioned, are applied to the element by using ngClass.

Coming to ngModel, we have already covered it in the previous blog (Data Binding Concepts).

### Installing BootStrap in an Angular Application:

Ok, let's talk about the use of BootStrap in our Angular application. As we know that usage of BootStarp will help us to style our application a lot better. For installation, fire this command in your terminal:

```markdown
 npm install Bootstrap
```

Include the necessarily required path to bootstrap.min.css in the **angular.json** file.

```json
"styles": [
              "src/styles.css",
              "node_modules/bootstrap/dist/css/bootstrap.min.css"
            ]
```

Restart the server, as we have committed changes in the main project folder and files. It is considered a good practice.

Thus we have successfully studied Attribute directives and also installed BootStrap in our application.