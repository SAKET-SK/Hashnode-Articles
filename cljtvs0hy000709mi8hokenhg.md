---
title: "Day 10: Angular - VIewChild Decorator  & Custom Directives"
datePublished: Sat Jul 08 2023 10:46:44 GMT+0000 (Coordinated Universal Time)
cuid: cljtvs0hy000709mi8hokenhg
slug: day-10-angular-viewchild-decorator-custom-directives
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/AxyVu2-chik/upload/ffb9addfd424977459090d9fba4e30e9.jpeg
tags: angularjs, frontend-development

---

In Angular, directives are a way to extend the behavior of HTML elements. They allow you to create reusable components or apply specific behavior to elements. There are three types of directives in Angular: custom directives, attribute directives, and structural directives.

Sometimes we need to create our Directives which function according to the code we write. We may cause the Custom Directives to function as we wanted to. Custom directives in Angular are user-defined directives that you can create to encapsulate specific behavior and reuse throughout your application. You can create custom directives using the "@directive" decorator provided by Angular. Let us try to create a custom directive that tends to change the color of a specific HTML element.

But before doing so, we will learn about @ViewChild decorator a bit. Here we will create 2 components (comp1 and comp2) and note that these 2 components are in the parent-child relationship. Also, there will be one-way communication between them. **Comp2 is a parent component of Comp1.**

The @ViewChild decorator in Angular is used to access a reference to a child component, directive, or element in the parent component. It allows the parent component to query and interact with elements or components that are declared within its template. Here we will write a code in which we will create a counter on a web page. It will contain two buttons, to increment and decrement the counter as we click on it. The interesting thing is, the **counter is a part of the child component** and **the buttons are a part of the parent component**. So we will see the usage of @ViewChild effectively.

Have a look at the "Comp1" child component HTML and TypeScript code:

```xml
<p>comp1 Child Component</p>
<strong>{{ myCounter }}</strong>
```

```typescript
import { Component, OnInit } from '@angular/core';
@Component({
  selector: 'app-comp1',
  templateUrl: './comp1.component.html',
  styleUrls: ['./comp1.component.css']
})
export class Comp1Component implements OnInit {
  myCounter: number = 0
  constructor() {}
  ngOnInit(): void {}

  incrementCounter(){
    this.myCounter++;
  }

  decrementCounter(){
    this.myCounter--;
  }
}
```

This child component houses the methods which are required to perform increment and decrement operations. Now we will create a parent component named "Comp2" which will have buttons to increment and decrement the value.

```xml
<p>comp2 Parent Component </p>
<button class="btn btn-success m-2" (click)="increment()">Increment Counter</button>
<button class="btn btn-danger m-2" (click)="decrement()">Decrement Counter</button>
<app-comp1></app-comp1>
```

The &lt;app-comp1&gt; signifies that the content of the "Comp1" will be displayed below after the buttons. We need to import the child Component in the TypeScript code.

```typescript
import { Component, OnInit, ViewChild } from '@angular/core';
import { Comp1Component } from '../comp1/comp1.component';
@Component({
  selector: 'app-comp2',
  templateUrl: './comp2.component.html',
  styleUrls: ['./comp2.component.css']
})
export class Comp2Component implements OnInit {
  // Referring to child component in HTML file
  @ViewChild(Comp1Component) cp: Comp1Component | undefined
  constructor() {}
  ngOnInit(): void {}

// Methods from the child component
  increment(){
    // console.log("Increment clicked")
    this.cp?.incrementCounter()
  }
  decrement(){
    this.cp?.decrementCounter()
  }
}
```

Now try and understand the flow. Firstly the HTML code of "Comp2" will be triggered. Later the contents of "Comp1" will be displayed as we have put the tag &lt;app-comp1&gt; in HTML. The buttons have been set with the "click" event; if they are clicked, will trigger the "increment()" and "decrement()" methods as specified. In the TypeScript file, we import the "Comp1" as we are going to need its reference. Here we use the "@ViewChild" decorator, to obtain the reference of the Child Component "Comp1". In the "increment()" and "decrement()" methods, we use the reference obtained from the child element, to access its methods, where we have written to actual code to increment and decrement the counter. Our application should look like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688794707282/9abd151e-a814-4c64-b4ff-30fae7548c42.png align="center")

These buttons function perfectly as do their increment and decrement function as intended. Now we will be focusing on the custom directives part.

---

To create a custom directive, we will type the following command into our command line:

```plaintext
ng generate directive <<directive-name>>
```

This will generate two files. In my case if I want to create a folder with the directive inside it:

```plaintext
ng generate directive custom-dir/changeColor
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688795012189/e8092bc1-02ad-4dc8-b3e7-45d1f402cad2.png align="center")

Inside our "change-color.directive.ts" file, we will have this code:

```typescript
import { AfterViewInit, Directive, ElementRef } from '@angular/core';
@Directive({
  selector: '[appChangeColor]'
})
export class ChangeColorDirective implements AfterViewInit {
  constructor(private ref: ElementRef) { }

  ngAfterViewInit() {
    this.ref.nativeElement.style.color = "red";
  }

  changeColor(newColor: any){
    this.ref.nativeElement.style.color = newColor;
  }
}
```

Note that, we can refer to this custom directive via the selector "appChangeColor". In the method "ngAfterViewInit", we are specifying that after our application has been initialized, it states what should be the color when our app starts. In the "changeColor()" method we can modify the color of elements as we obtain from clicking the radio buttons we designed. We will create a component called "my-color" to demonstrate the work. In the HTML of that component, we will have this as the MAIN code:

```xml
<h2 appChangeColor>Change the Color</h2>
<hr>
<h4 appChangeColor>H4 with appChangeColor custom directive</h4>
<div appChangeColor>div with appChangeColor custom directive</div>
<p appChangeColor>p with appChangeColor custom directive</p>

<input type="radio" name="color" (click)="setColor('red')"> Red
<input type="radio" name="color" (click)="setColor('green')"> Green
<input type="radio" name="color" (click)="setColor('blue')"> Blue
```

```typescript
import { Component, OnInit, QueryList, ViewChild, ViewChildren } from '@angular/core';
import { ChangeColorDirective } from 'src/app/custom-dir/change-color.directive';
@Component({
  selector: 'app-my-color',
  templateUrl: './my-color.component.html',
  styleUrls: ['./my-color.component.css']
})

export class MyColorComponent implements OnInit {
    // It referes to only first child element, not others
    // @ViewChild(ChangeColorDirective) clr: ChangeColorDirective | undefined;

    // Will refer to all matching child elements
    @ViewChildren(ChangeColorDirective) allChildren: QueryList<ChangeColorDirective> | undefined;
}
setColor(color: any){
    // this.clr?.changeColor(color)   // In case of @ViewChild, use this
    this.allChildren?.forEach(e => {  // In case of @ViewChildren, use this
       e.changeColor(color)
     })
}
```

Now understand the difference between @ViewChild and @ViewChildren directives. In the HTML code above, we can see the hierarchy of child elements as:

```plaintext
h2 -> h4 -> div -> p
```

If I use the @ViewChild decorator code like this:

```typescript
export class MyColorComponent implements OnInit {
    @ViewChild(ChangeColorDirective) clr: ChangeColorDirective | undefined;
    setColor(color: any){
        this.clr?.changeColor(color)
    }
}
```

Then see the output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688812184374/4016f39e-df94-4f29-9c3d-ced8e56e7c5f.png align="center")

This is what we see at the start. When we click on the buttons below, see the magic:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688812251913/dd162c44-01fc-4a7a-92aa-cb91b60b4682.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688812318943/cda7ece4-2674-44cd-9b25-949e2a3175c3.png align="center")

As you can see, the effect of the @ViewChild decorator is only on the first child, but we need to change the color of all children in this case. So, instead of the @ViewChild directive, we will use @ViewChildren.

```plaintext
// ViewChild will always refer to the first child element (which it finds)
@ViewChild(ChangeColorDirective) clr: ChangeColorDirective | undefined

// ViewChildren will refer to all the matching child elements
@ViewChildren(ChangeColorDirective) allChildren: QueryList<ChangeColorDirective> | undefined
```

So to do that, we need to update our code so that we can use the @ViewChildren as discussed in the above previous MAIN code. By using the @ViewChildren code, we obtain this output:  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688813011057/27f69c7d-c167-4908-b1b4-726138178032.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688813047150/63605a58-563b-4307-86d9-e902cf615340.png align="center")

It will be Red by default, but now you can observe the difference crystal clear.

Thus we have successfully created our custom directive, in which we changed the color of specific elements. We also learned about the difference between @ViewChild and @ViewChildren decorator. We will be focusing on other important topics in our later blogs.