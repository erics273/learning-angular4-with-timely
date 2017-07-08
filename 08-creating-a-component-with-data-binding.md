# Your Second Component (Part 2 of 3): Data and Action Binding

You remember from a conversation that you had that
there are two types of binding in JavaScript
frameworks: data binding and action binding.

**Data binding** occurs when a UI framework like
Angular automatically associates the value of a
JavaScript variable with what's in a form field.

**Action binding** occurs when a UI framework ties
together an action, like a person clicking a button,
with a JavaScript method.

**Data rendering** occurs when a value in a variable
of the component changes and Angular updates the view
with the value.

You recognize that there are four things that you
want to bind in the `login-card` component.

1. You want to bind the value of the `username` input
   field to a variable in the `LoginCardComponent`
   class;
1. You want to bind the value of the `password` input
   field to a variable in the `LoginCardComponent`
   class;
1. You want to use those values to bind to the
   `disabled` attribute of the "WATCH TIME" button so
   that it's enabled when both fields have something
   typed in them; and,
1. You want to call a method in the
   `LoginCardComponent` when a person clicks the
   "WATCH TIME" button.

## Reviewing the Component for Data Binding

Right now, the `login-card.component.ts` file looks
like this:

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

The `LoginCardComponent` class is the controller for
the component. That means all of the data from the
view and the actions that people will take should be
handled in the `LoginCardComponent` class.

[callout-info]
In the Model-View-Controller pattern, the controller
acts as the glue between the data in the model (which
you don't have, yet) and the view (which you just
created in the `login-card.component.html` file).
[/callout-info]

## Data Binding the Input Fields

Right now, the `login-card.component.html` file looks
like this:

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="form-line">
    <input type="text" placeholder="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password">
  </div>
</div>
<div class="actions">
  <button class="cta">Watch time</button>
</div>
```

To bind an `input`'s value to a variable of the
controller of the component, you use the `ngModel`
attribute. In the case of your form, you look at the
`username` `input` field.

```html
<!-- Plain old HTML -->
<input type="text" placeholder="username">
```

You change that to use the `ngModel` attribute with
the name of the property that you want to set. You
decide that you want to bind it to the `username`
property of the `LoginCardComponent` class. This
changes the `<input>` field to look like this.

```html
<!-- Now, with Angular data binding because of "ngModel"! -->
<input type="text" placeholder="username" [(ngModel)]="username">
```

When you save the file, your brow furrows because you
now see an error in the developer tools that reads:

> Can't bind to 'ngModel' since it isn't a known
> property of 'input'

A little [duckduckgo](http://duckduckgo.com) magic
leads you to [a
solution](http://jsconfig.com/solution-cant-bind-ngmodel-since-isnt-known-property-input/)
to the problem, but not the *reason*. You do some more
searching and cobble together why your application
broke by trying to use the `ngModel` attribute. You
take a small detour from data binding to set up the
application to use `ngModel` on `<input>`s.

### Backing Up: Readying the Application for Data Binding

In your reading, you find that when the Angular team
took on the challenge to create the next version of
Angular, to evolve it from 1.X to 2.X, they decided to
modularize the different portions of functionality so
you only import the stuff that you need to use.

You open the `app.module.ts` and see that the only
functionality-enhancing Angular module that your
application uses, right now, is the `BrowserModule`.
You want to start using form elements which means that
you are going to need the `FormsModule`'s
functionality added to the application. You modify the
`app.module.ts`.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

// Import the FormsModule so you can use form-based
// data binding
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { RegistrationCtaComponent } from './registration-cta/registration-cta.component';
import { LoginCardComponent } from './login-card/login-card.component';

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent,
    LoginCardComponent
  ],
  imports: [
    BrowserModule,

    // Add the functionality of the external Angular
    // forms module to your application module.
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

When you make this change and save the file, the error
goes away. You smile and get back to data binding the
`<input>`s.

### Back to Data Binding the Input Fields

Now, you bind the `password` field so that your
component's HTML now looks like this:

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="form-line">

    <!-- Look at this lovely data binding! -->
    <input type="text" placeholder="username" [(ngModel)]="username">

  </div>
  <div class="form-line">

    <!-- Look at this lovely data binding! -->
    <input type="password" placeholder="password" [(ngModel)]="password">

  </div>
</div>
<div class="actions">
  <button class="cta">Watch time</button>
</div>
```

[callout-info]
You just used Angular to monitor those `<input>`
fields. Now, whenever someone types something into
them, it will automagically get stored in the
`username` and `password` properties of the
`LoginCardComponent` instance!
[/callout-info]

For right now, you just accept the "`[()]`" that wraps
the `ngModel`. You saw that's how it was supposed to
be done and, after you get this component bound, you
will spend the time understanding that weird
punctuation.

## Data Binding the Button's Disabled State

A little bit of research show that you can control
the value of the `disabled` property of the `<button>`
using Angular.

In your case, you figure that you want the button
disabled if the `login.username` or `login.password`
values are empty, that is, if they have a length of 0.
Formally, you want to

> Disable the "WATCH TIME" button if the length of the
> `login.username` value is 0 *or* if the length of
> the `login.password` value is 0.
>
> In JavaScript logic:
> login.username.length === 0 || login.password.length === 0

Because this is logic based on values in the
component, you decide that putting the logic for this
in the `LoginCardComponent` class. In
`login-card.component.ts`, you add a getter value
named `disableButton` and the `private` fields that
will allow you to access those properties from the
HTML.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  // The TypeScript compiler requires that you have
  // these. You set them to default values of empty
  // strings.
  private username = '';
  private password = '';

  constructor() { }

  ngOnInit() {
  }

  // A getter that allows that you will bind to the
  // button to make sure that it's disabled properly.
  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

}
```

Now, you use this new getter in the view, in
`login-card.component.html`.

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">

  <!-- When disableButton is true, it disables the button! -->
  <button class="cta" [disabled]="disableButton">Watch time</button>

</div>
```

You smile after the page refreshes because, now, the
"WATCH TIME" button is disabled until you type some
values into both the username and password fields!

Now, you see that the `disabled` property is
surrounded by some "`[]`". You feel slightly
uncomfortable because you're using things you don't
fully understand, yet. You just know they work. Now,
you've used "`[(...)]` and `[]` when binding in a
component template. You promise yourself that, after
the component can do something when you click the
button, you'll figure out what this punctuation means.

## Binding the Button Click

When someone clicks that button, you want the
component to submit the credentials that the person
has provided. You just want to wire up that event
handler in such a way that you are assured that
everything works in the way that you expect. Then, you
can get on with the business of actually logging in
the person who supplied the credentials. You realize
that you need a method to call for the button click.
You put that on the `LoginCardComponent` class.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  private username = '';
  private password = '';

  constructor() { }

  ngOnInit() {
  }

  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

  // Here's a method that the button can use when a
  // person clicks the button.
  submitCredentials() {
    console.log('username:', this.username);
    console.log('password:', this.password);

    this.username = '';
    this.password = '';
  }

}
```

Right now, your method just prints out the values that
the person supplied and sets the values of the
variables to empty strings which clear the two
`<input>` fields. To get the `<button>` to call the
method, you bind the Angular `click` event handler.

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">

  <!-- Now, the button will be disabled based on
       the username and password values, and will call
       the submitCredentials() method when the button
       is active and a person clicks it. -->
  <button class="cta"
          [disabled]="disableButton"
          (click)="submitCredentials()">Watch time</button>
</div>
```

Your smile becomes even larger after the page
refreshes. You type in some text into the username and
password fields of the form, click the "WATCH TIME"
button or hit Enter, and the console messages appear
with the values of the `username` and `password`
properties that the `<input>` fields are bound to!

But, now, your smile fades because it's time to figure
out the "`[()]`", "`[]`", and "`()`" that you've used
to get this component working.

## Angular Binding Punctuation

You make some notes while reading the [*Binding
Syntax: An
Overview*](https://angular.io/guide/template-syntax#binding-syntax-an-overview)
section of the *Template Syntax* entry of the Angular
documentation and run across a table that explains the
different punctuation that you copy into your notes
for quick reference.

| Punctuation               | Binding Type           | Explanation                                                                                                                                         |
|---------------------------|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `[prop]="expression"`     | One-way data binding   | Take the JavaScript *expression*, evaluate it in context of the instance of the component class, and set the value of the `prop`erty to that value. |
| `(event)="expression"`    | One-way action binding | When *event* occurs, evaluate the *expression* in context of the instance of the component class.                                                   |
| `[(target)]="expression"` | Two-way data binding   | Make sure that the values of `target` and the *expression* remain in sync.                                                                          |
| `{{ expression }}`        | One-way data rendering | Put the value of *expression* into the template.                                                                                                    |

You've seen the first three and kind of understand
what's happening with those.

For the first entry, the one-way data binding, you
used `[disabled]="disableButton"` which sets the
boolean property `disabled` to whatever the value of
`disableButton` is with respect to the instance of the
component class.

For the second entry, the one-way action binding, you
used `(click)="submitCredentials()"` to respond to
**click** events of the button and call the
`submitCredentials()` method on the instance of the
component class.

For the third entry, the two-way data binding, you
used `[(ngModel)]="password"` to synchronize the value
of the `password` attribute with the value of the
`<input>` field. When the value changes in the
`<input>`, the two-way binding sets the value in the
instance of the component class. When the value of the
bound field changes in the component class, like when
a person clicks the button, the two-way binding
updates the value in the `<input>`.

You haven't used that last kind of binding, yet, the
one-way data rendering syntax `{{ expression }}`. But,
you know that you'll soon be showing data in the view
and you'll have plenty of opportunity to try that out.

## What Did You Do?

You rub your eyes a little bit because this was a lot
of work and a lot of new stuff. Breaking it down, you
learned about binding in many different ways.

You used `[(ngModel)]` to *bind controller properties
to form fields*.

You used `[disabled]="disableButton` to *control the
disabled state of a button*. You can use this to
control many of the properties on HTML elements.

You used `(click)="submitCredentials()"` to *bind a
button click to a component method*.
