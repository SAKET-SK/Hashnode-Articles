---
title: "Day 6 : Angular - All about Pipes"
datePublished: Thu Feb 16 2023 17:06:31 GMT+0000 (Coordinated Universal Time)
cuid: cle7ctfyn000709mf79wvhqo9
slug: day-6-angular-all-about-pipes
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/scUBcasSvbE/upload/f0e8a22b482df317fe935a2c697f3718.jpeg
tags: mean, angularjs, frontend-development, angular-material

---

"Pipes are **simple functions to use in template expressions to accept an input value and return a transformed value**. Pipes are useful because you can use them throughout your application, while only declaring each pipe once."

To create a pipe, we will use this command in our terminal.

```markdown
ng generate pipe <pipe-name>
```

Alternatively, you can also type "ng g pipe".

We will see the following pipes in this article:

* String Pipe
    
* JSON Pipe
    
* Currency Pipe
    
* Number Pipe
    
* Date Pipe
    
* Custom Pipe -&gt; Which we will create on our own; the rest are in-built
    

### String Pipe:

A string pipe is used to perform some operations on strings. We will pass the string to the pipe and then some kind of operation will be performed on it. Let us see the below example.

```xml
<h4>String Pipes</h4>
<div>{{ message | lowercase }}</div>
<div>{{ message | uppercase }}</div>
<div>{{ message | titlecase }}</div>
<div>{{ message | slice: 10:25 | uppercase}}</div>
```

In the TS file, we have the message variable declared:

```typescript
 message = "Starting Angular Pipes"
```

And the output we get is:  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676560802099/ce127f41-18a3-4515-af2e-5d04dc343f6c.png align="center")

As you can see on line one, we have asked the pipe to make the message in lowercase format. Similarly, there are other operations to modify the string as shown. Apart from case operations, we can also slice the string and simultaneously make it uppercase too; that means we can assign more than one pipe also.

### JSON Pipe:

It takes the data in JSON format and then performs certain operations on it. Let us look at the below example. We have the following data in TS file in JSON format which we will pass to the JSON pipe.

```json
emp = {
    id: 1001,
    name: "ABC",
    post: "Engineer",
    salary: 35000,
    address: "Pune"
  }
```

In HTML, to use JSON pipe, we will write:

```xml
<h4>JSON Pipes</h4>
<strong>{{emp | json}}</strong>
```

And the output we get is:  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676561206006/cb4e003c-9ca1-46ed-bdaf-66bd0c649341.png align="center")

As you can see, we have passed the word "json" to it along with the name of the array we used in the JSON file.

### Currency Pipe:

A currency pipe is used to perform currency-related operations for instance working with currency signs. Take the example of any e-commerce site such as Amazon. For the currency sign to display on the UI of the website, you wouldn't change the sign manually all around the hundreds of HTML files. Using a simple pipe, we can specify the currency type depending on the region the user is situated. As location changes, the pipe tends to be updated and prices are altered too.

```typescript
price = 9999  // TS file
```

```xml
<h4>Currency Pipes</h4>
<div><strong>{{ price | currency }}</strong></div>
<div><strong>{{ price | currency:"INR" }}</strong></div>
<div><strong>{{ price | currency:"GBP" }}</strong></div>
```

The first row will be the one with a default value. In the second and third lines, we are specifying the type of currency. We will get the following output.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676565204929/374216d8-6515-4aab-a36c-09014387f4cd.png align="center")

Have a look at the changing currency signs. That is what we are doing, modifying the data.

### Number Pipe:

As the name suggests, we can perform operations on numbers regarding decimal places, commas, etc. Take a look at an example below:

```xml
<h4>Number Pipes</h4>
<div>{{ 910 | number }}</div>
<div>{{ 910 | number:'2.2' }}</div>  <!--Before and after decimal places. If we put 5.2 as first number, the output: 00910.00-->
<div>{{ 910 | number:'5.2' }}</div>
```

And the output we obtain is:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676565415777/32cbb9c9-ebb6-4283-a47e-4be5b7b38e87.png align="center")

Number pipes are useful in representing numbers in different formats.

### Date Pipe:

Similar to numbers, the format of Date is also different in different parts of the world. Well to make things compatible for users depending on their region, we can also represent data in different formats to suit their needs. Take a look at the below example:

```typescript
myNewDate = new Date()   // Takes the current date and time instance
```

```xml
<h4>Date Pipes</h4>
<div><strong>{{ myNewDate }}</strong></div>
<div><strong>{{ myNewDate | date }}</strong></div>
<div><strong>{{ myNewDate | date:'shortDate' }}</strong></div>
<div><strong>{{ myNewDate | date:'longDate' }}</strong></div>
<div><strong>{{ myNewDate | date:'mediumDate' }}</strong></div> <!--Same as only date-->
```

And the output we obtain is:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676565729571/0bb92ed6-3aa8-4252-ad48-1c87a2888914.png align="center")

Shortdate and Longdate are different ways to represent data too. The first line output will differ from region to region. By default, the date is in MediumDate format.

### Custom Pipe:

Ok, enough of playing with the in-built pipes. Let us make our pipe and perform the operations we desire. Fire up the following command in your terminal.

```markdown
ng generate pipe ../custom-pipe/gender  
```

Please note you can have any name for your pipe. I have used the "custom-pipe" folder and in that I have named my pipe as "gender" in my case. This will give us 2 files:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676566074337/15a5ded0-0448-4a39-b0c7-464360e9e55e.png align="center")

Please note, we will not be doing anything with the spec.ts file. Our main code will be inside the .ts only.

What we will be doing is we will create a table of students. If the student is male, automatically "Mr." should be joined in front of the name. Likewise for females, "Ms." should be appended at the beginning.

This is the data from the.ts file which we will be using

```typescript
students = [
    {id:1001, name:"Allan", gender:"Male", address:"London", contact:"9898989898"},
    {id:1002, name:"Bob", gender:"Male", address:"Paris", contact:"94565789898"},
    {id:1003, name:"Carol", gender:"Female", address:"Spain", contact:"9898980008"},
    {id:1004, name:"Davis", gender:"Female", address:"London", contact:"1298989898"},
    {id:1005, name:"Elmer", gender:"Male", address:"USA", contact:"5410989898"},
    {id:1006, name:"Frank", gender:"Male", address:"Germany", contact:"9999989898"},
    {id:1007, name:"Grace", gender:"Female", address:"London", contact:"1298981680"}
  ]
```

In HTML, we make arrangements to display the table.

```xml
<h4>Custom Pipes</h4>
<table class="table table-striped">
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Gender</th>
            <th>Address</th>
            <th>Contact</th>
        </tr>
    </thead>
    <tbody>
        <tr *ngFor = "let s of students">
            <td>{{ s.id }}</td>
            <td>{{ s.name | gender:s.gender }}</td>
            <td>{{ s.gender }}</td>
            <td>{{ s.address }}</td>
            <td>{{ s.contact }}</td>
        </tr>
    </tbody>
</table>
```

As you can see we have applied the name of our pipe, which in our case is "gender" specifically applied to the name, as we have to perform an append operation on that particular field.

Later in the class of file **gender.pipe.ts** file, we write:

```typescript
transform(value: unknown, genderValue: string): unknown {
    if(genderValue.toLowerCase() == "male"){
      return "Mr. " + value;
    }
    else{
      return "Ms. " + value;
    }
  }
```

The logic is simple as seen. Now let us have a look at the output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676566792403/865ce12c-bb4c-46a7-8994-18a801318414.png align="center")

See the name column. We didn't specify "Mr." or "Ms.". It has been updated according to our logic code which we wrote in the .ts file of our gender pipe.

Similarly, we can create many such custom pipes and write the logic according to which we want the pipe to present the data to us.

Thus we learnt about pipes in angular.