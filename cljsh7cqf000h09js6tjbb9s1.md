---
title: "Day 9: Angular - Angular Forms"
datePublished: Fri Jul 07 2023 11:10:59 GMT+0000 (Coordinated Universal Time)
cuid: cljsh7cqf000h09js6tjbb9s1
slug: day-9-angular-angular-forms
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/wgcUx0kR1ps/upload/a71102a3c4b0d0f966a25a6a427cdbd0.jpeg
tags: angularjs, frontend-development, form-validation

---

An Angular Form is a mechanism provided by the Angular framework to handle user input and perform data validation in Angular applications. It allows you to create interactive and dynamic forms that collect data from users and respond to their input.

There are two types of forms we will see:

1. Template Driven Form:- Simple Form, Simple Validations
    
2. Reactive Form:- Complex Form, Complex Validations
    

We will look into both of these concepts in detail with the code.

### Template Driven Form:

Template-driven forms are based on directives and are easier to set up. In a template-driven form, **you define the form structure in the HTML template and use directives like "ngModel" to bind form controls to properties in the component.** The form controls are automatically synchronized with the component's data model. Let us create a registration form for some institutes that contains First name, Last Name, Email, Phone Number, A list of courses, etc. The first three fields we will use for demonstration; we will sort of upgrade the form with necessary validations.

```xml
<form #regForm="ngForm" (ngSubmit)="getData(regForm.value)">
    <div>
        <label>Enter First Name : </label>
        <input type="text" id="fname" name="fname" ngModel required>
    </div>
    <div>
        <label>Enter Last Name : </label>
        <input type="text" id="lname" name="lname" ngModel required>
    </div>
    <div>
        <label>Enter Email : </label>
        <input type="email" id="email" name="email" ngModel required>
    </div>
    <div>
        <button type="submit" [disabled]="!userForm.valid">Submit</button>
    </div>
</form>
```

Now understand the basic structure of this Template Driven Form. The name of our form here is "#regForm". As we have mentioned in the first line after the "ngSubmit", the method "getData(regForm.value)". We need to write that method in our "ts" file.

```typescript
getData(val:any){
    console.log(val)
  }
```

The method has one argument called regForm.value, which means that it will get all the values we entered in the form, and as specified in the method, it will log that data onto the console.

One more thing, the button as coded in HTML form will remain disabled until we will fill in all other fields. Let us do some validations and upgrade our form functionality. We will work on one by one field.

```xml
<div>
    <label>Enter First Name : </label>
    <input type="text" id="fname" name="fname" [pattern]="[a-zA-Z ]{3,30}" ngModel required>
</div>
```

Now we have sort of upgraded the field with one more validation with REGEX expression. The pattern specified translates as the name should contain letters within the range of small A to small z and Capital A to Captial Z. Minimum it should be of length 3 and at maximum it should be at length 30. Now let us write the code to display the error message in case the validation fails.

```xml
<div style="color: red" *ngIf="fName.invalid && fName.touched">
                <div *ngIf="fName.errors?.['required']">
                    First Name is required
                </div>
                <!-- ? = SAFE NAVIGATION OPERATOR -->
                <div *ngIf="fName.errors?.['pattern']">
                    First Name must contain minimum 3 and maximum 30 letters
                </div>
            </div>
```

The above code snipped roughly states that if the "fname" field is invalid AND the "fname" field is touched (Clicked upon once to write some text into it), then proceed to check for errors; as the error can be one of the above. If the error is regarding missing out on the required field, print out that particular text in red color. If the error is regarding the mismatch in patterns, then print out the corresponding error message.

Repeat this for the last name field too. So the updated code must look like this:

```xml
<div>
    <label>Enter First Name : </label>
    <input type="text" id="fname" name="fname" [pattern]="[a-zA-Z ]{3,30}" ngModel required>
</div>
<div style="color: red" *ngIf="fName.invalid && fName.touched">
    <div *ngIf="fName.errors?.['required']">
        First Name is required
    </div>
    <!-- ? = SAFE NAVIGATION OPERATOR -->
    <div *ngIf="fName.errors?.['pattern']">
        First Name must contain minimum 3 and maximum 30 letters
    </div>
</div>
```

There will be no change in the "ts" file. After doing such basic changes, you will be able to see the following output.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688619566754/30782c23-37ac-46e1-9c1b-335e72be5411.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688619604424/57cdddbc-39e3-433a-bb29-572115ccdf76.png align="center")

Here I have given my form some bootstrap styles. You may also do so if you want to beautify your application to some extent. I will repeat the same code for the field "last name" too. For the "email" field, we will have to change the "\[pattern\]".

```xml
<!--REGEX pattern for email-->
pattern="[a-z0-9]+@[a-z]+\.[a-z]{2,3}"
```

Now let us add some more fields namely password, phone number, and a drop-down list.

```xml
<form #regForm="ngForm" (ngSubmit)="getData(regForm.value)">
    <!--EARLIER CODE HERE-->
    <div>
        <label>Enter Password : </label>
        <input type="password" id="password" name="password" ngModel required>
    </div>
    <div>
        <label>Enter Phone Number : </label>
        <input type="tel" id="phone" name="phone" ngModel required>
    </div>
    <div>
        <label>Select a course : </label>
        <select name="course" #course="ngModel" class="form-control" ngModel required>
            <option value="">Select a Course</option>
            <option [value]="c" *ngFor="let c of listcourses">{{ c }}</option>
        </select>
    </div>
    <div>
        <button type="submit" [disabled]="!userForm.valid">Submit</button>
    </div>
</form>
```

Have a look at the **"#course = ngModel"** statement. Here we are assigning a reference name to the control (#refName). Then we add the ngModel directive to the control (to register this control with Angular). In this way, If we want to know the status of our control, we can obtain it.

Now for the drop-down list, we need to add this line to the "ts" file.

```typescript
courses: string[] = ['C/C++','Java','HTML/CSS/JS','Python','AI/ML']
```

In this way, we can data to the drop-down list.

After adding all the necessary validations, our full code must look like this:

```xml
<form #regForm="ngForm" (ngSubmit)="getData(regForm.value)">
    <div>
        <label>Enter First Name : </label>
        <input type="text" #fName="ngModel" name="fname" [pattern]="[a-zA-Z ]{3,30}" ngModel required>
    </div>
    <div style="color: red" *ngIf="fName.invalid && fName.touched">
        <div *ngIf="fName.errors?.['required']">
            First Name is required
        </div>
        <!-- ? = SAFE NAVIGATION OPERATOR -->
        <div *ngIf="fName.errors?.['pattern']">
            First Name must contain minimum 3 and maximum 30 letters
        </div>
    </div>
    <div>
        <label>Enter Last Name : </label>
        <input type="text" #lName="ngModel" name="lname" [pattern]="[a-zA-Z ]{3,30}" ngModel required>
    </div>
    <div style="color: red" *ngIf="lName.invalid && lName.touched">
        <div *ngIf="lName.errors?.['required']">
            Last Name is required
        </div>
        <!-- ? = SAFE NAVIGATION OPERATOR -->
        <div *ngIf="lName.errors?.['pattern']">
            Last Name must contain minimum 3 and maximum 30 letters
        </div>
    </div>
    <div>
        <label>Enter Email : </label>
        <input type="email" #email="ngModel" name="email" pattern="[a-z0-9]+@[a-z]+\.[a-z]{2,3}" ngModel required>
    </div>
    <div style="color: red" *ngIf="email.invalid && email.touched">
        <div *ngIf="email.errors?.['required']">
            Email is required
        </div>
        <div *ngIf="email.errors?.['pattern']">
            Email must be valid
        </div>
    </div>
    <div>
        <label>Enter Password : </label>
        <input type="password" #password="ngModel" name="password" pattern="[a-zA-Z0-9 ]{7,20}" ngModel required>
        <div style="color: red" *ngIf="password.invalid && password.touched">
            <div *ngIf="password.errors?.['required']">
                Password is required
            </div>
            <div *ngIf="password.errors?.['pattern']">
                Password must contain atleast 7-8 characters
            </div>
        </div>
    </div>
    <div>
        <label>Enter Phone Number : </label>
        <input type="tel" #phone="ngModel" pattern="[0-9]{10}" name="phone" ngModel required>
        <div style="color: red" *ngIf="phone.invalid && phone.touched">
                <div *ngIf="phone.errors?.['required']">
                    Phone Number is required
                </div>
                <div *ngIf="phone.errors?.['pattern']">
                    Phone Number must contain 10 characters
                </div>
            </div>
        </div>
    <div>
        <label>Select a course : </label>
        <select name="course" #course="ngModel" class="form-control" ngModel required>
            <option value="">Select a Course</option>
            <option [value]="c" *ngFor="let c of courses">{{ c }}</option>
        </select>
    </div>
    <div>
        <button type="submit" [disabled]="!userForm.valid">Submit</button>
    </div>
</form>
```

This is what our code looks like Template Driven Form, applied with basic validations. To summarize, we write the basic form code in HTML, bind it with Angular using "ngModel" and apply validations.

---

### Reactive Forms:

Reactive Forms in Angular provide a more explicit and flexible approach to handling forms. They involve creating form controls, form groups, and form arrays programmatically in the component class. Here's an example code to demonstrate how to create a reactive form:

```xml
<form [formGroup]="empData" (ngSubmit)="getData(empData.value)">
    <div>
        <label for="">Enter First Name : </label>
        <input type="text" class="form-control" formControlName="fname" placeholder="Enter First Name">
    </div>
    <div>
        <label for="">Enter Last Name : </label>
        <input type="text" class="form-control" formControlName="lname" placeholder="Enter Last Name">
    </div>
    <div>
        <label for="">Enter Email : </label>
        <input type="text" class="form-control" formControlName="email" placeholder="Enter Email">
    </div>
    <div>
        <label for="">Enter Password : </label>
        <input type="text" class="form-control" formControlName="password" placeholder="Enter Password">
    </div>
    <div>
        <label for="">Enter Contact Number : </label>
        <input type="text" class="form-control" formControlName="phone" placeholder="Enter Phone Number">
    </div>
    <div>
        <label for="">Enter Course : </label>
        <select name="course" formControlName="courses" class="form-control">
                    <option value="">Select a Course</option>
                    <option [value]="c" *ngFor="let c of courses">{{ c }}</option>
        </select>
    </div>
    <div>
        <label class="form-check-label">
            <input type="checkbox" name="term" formControlName="terms" class="form-check-input">
                I accept the terms and conditions
            </label>
    </div>
    <div>
        <button type="submit" class="btn btn-primary" [disabled]="empData.invalid">Submit</button>
    </div>
</form>
```

Carefully have a look at the "formControlName" field. Before going any deeper have a look at the "ts" file.

```typescript
import { FormBuilder, FormControl, FormGroup, Validators } from '@angular/forms';
export class RfComponent implements OnInit {
    empData: any
    courses: string[] = ['C/C++','Java','HTML/CSS/JS','Python','AI/ML']
    constructor(private fb: FormBuilder) {}
    ngOnInit(): void {
    // THERE ARE 2 options; you may use ANY ONE of these
    // Option 1 :- Create FormGroup object followed by FormControl objects
        this.empData = new FormGroup({
            fname: new FormControl('',[Validators.required, Validators.pattern('[a-zA-Z ]{3,30}')]),
            lname: new FormControl('',[Validators.required, Validators.pattern('[a-zA-Z ]{3,30}')]),
            email: new FormControl('',[Validators.required, Validators.pattern('[a-z0-9]+@[a-z]+\.[a-z]{2,3}')]),
            password: new FormControl('',[Validators.required, Validators.pattern('[a-zA-Z0-9 ]{7,20}')]),
            phone: new FormControl('',[Validators.required, Validators.pattern('[0-9]{10}')]),
            courses: new FormControl('',[Validators.required]),
            terms: new FormControl('',[Validators.requiredTrue]) // This is required for radio button and checkbox
        })

    // Option 2 :- Using FormBuilder to create reactive forms
    // Use dependency injection for this. Means, pass the value in constructor. We will get object directly, not have to create it.
        this.empData = this.fb.group({
            fname: ['',[Validators.required, Validators.pattern(namePattern)]],
            lname: ['',[Validators.required, Validators.pattern(namePattern)]],
            email: ['',[Validators.required, Validators.pattern('[a-z0-9]+@[a-z]+\.[a-z]{2,3}')]],
            password: ['',[Validators.required, Validators.pattern('[a-zA-Z0-9 ]{7,20}')]],
            phone: ['',[Validators.required, Validators.pattern('[0-9]{10}')]],
            courses: ['',[Validators.required]],
            terms: ['',[Validators.requiredTrue]]
        });
    }
    getData(val:any){
    console.log(val)
  }
}
```

Let us summarize now. To create a Reactive Form, we are making the groups of forms using "FormGroup". Here we are simply registering the Form controls as well as passing the validations directly alongside it while registering. But while using the "FormBuilder", we need to use dependency injection for it to use. To do so, import it first and pass it onto the constructor as mentioned above. The names of the Form Controls should match the ones we give in the HTML code; as in HTML we are binding these control via the "formControlName".

For each form control, we specify a key (e.g., name, email, password) and its initial value. We can also specify one or more validators for each form control. In this example, we use the "Validators.required()" validator for the name and email fields, and additionally the "Validators.email()" validator for the email field. For the password field, we use both the"Validators.required()" validator and the "Validators.minLength(6)" validator to ensure it has a minimum length of 6 characters.

Now let us modify our HTML code so that in case of any validation error, the message should pop up. This is entirely different as compared to Template Driven Form. For reference, check the above code when we coded the Template Driven Form validation.

```xml
<div class="alert alert-danger" *ngIf="empData.get('fname').invalid && empData.get('fname').touched">
    <div *ngIf="empData.get('fname').errors.required">
        First Name is required
    </div>
    <div *ngIf="empData.get('fname').errors.pattern">
        First Name must contain min 3 and max 30 characters
    </div>
</div>
```

As we can see, "empData" is the name of the form, and the get() method is used to obtain its value. Later by using Angular directives, we try to display an appropriate error message if anything wrong has been triggered. You may use a similar validation code snippet for the rest of the other controls.

So to conclude, Template-driven forms are based on directives and are easier to set up. In a template-driven form, you define the form structure in the HTML template and use directives like "ngModel" to bind form controls to properties in the component. The form controls are automatically synchronized with the component's data model. Reactive Forms offer more flexibility and control over form handling and validation. They are particularly useful when dealing with complex forms or dynamic form controls.