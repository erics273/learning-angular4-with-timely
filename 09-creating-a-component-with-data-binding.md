# Your Second Component (Part 3 of 3): Using AJAX

Now is the time to get this page talking to the
server!

Angular has it's own AJAX service, defined in the
`HttpModule` and provided by the `Http` class. You
decide that you should use that to talk to the server.

You open the `login-card.component.ts` file and review
what you have in there.

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

  submitCredentials() {
    console.log('username:', this.username);
    console.log('password:', this.password);

    this.username = '';
    this.password = '';
  }

}
```

## Getting the `Http` Service Into Your Component

You want to use the `Http` service that Angular
provides to make AJAX calls. You want to *depend* on
that service. Moreover, you want to ask Angular to
give the `Http` service to your component, to
*inject* it into your component so that you can use
it.

This is known as **dependency injection**.

[callout-info]
**Dependency injection** is when you ask a framework,
like Angular, to provide something to you so that you
don't have to figure out how to get it yourself.

Angular has a lot of things that it can provide to
you, `Http` being just one. As you continue your
exploration of Angular, you'll run into more.
[/callout-info]

To use dependency injection, you need to do two
things:

1. Import any Angular module in your application
   module that defines the thing you want (if
   applicable); and,
1. Define a constructor parameter with the type that
   you want injected.

First up, you need to import the `HttpModule` and add
it to your application module. You open
`app.module.ts` and make the following additions.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';

// Import the HttpModule that defines the Http service
import { HttpModule } from '@angular/http';

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
    FormsModule,

    // Import the HttpModule into your application
    // module
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Now, you change the `LoginCardComponent` class'
constructor to ask for an instance of the `Http`
service.

```typescript
import { Component, OnInit } from '@angular/core';
import { Http } from '@angular/http';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  private username = '';
  private password = '';

  // Get Angular to inject the Http service into your
  // class when it creates your component. Now, you
  // can use it to make AJAX calls!
  constructor(private http: Http) { }

  ngOnInit() {
  }

  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

  submitCredentials() {
    console.log('username:', this.username);
    console.log('password:', this.password);

    this.username = '';
    this.password = '';
  }

}
```

## Using the `Http` Service to Authenticate

You want to use the `Http` service to make a call to
authenticate the person logging in. You make sure that
you have Michaela's application running, pull up a
browser, log in using "maria" as the username and
"maria" as the password, then navigate a new browser
to the API documentation at
[http://localhost:5000/swagger-ui.html](http://localhost:5000/swagger-ui.html).
There you see the Session API Controller that has a
PUT method to `/api/session/mine` that expects a JSON
payload containing the "username" and "password".
Michaela mentioned that she had set up the API to set
a cookie on a GET request that would act as a
[CSRF token](https://angular.io/guide/security#xsrf)
so you probably need to get that cookie set, first,
with a GET call.

You delete the contents of the `submitCredentials`
method of the `LoginCardComponent` class in your
component's `.ts` file and use the `Http` service to
make a call to the API endpoint. You put in the
following code:

```javascript
submitCredentials() {
  // Create the payload to send to the server
  const payload = {
    username: this.username,
    password: this.password
  };

  // Tell the AJAX request to send cookies
  const options = {
    withCredentials: true
  };

  // Put the URLs in convenience variables
  const cookieUrl = 'http://localhost:5000/api/clients';
  const sessionUrl = 'http://localhost:5000/api/session/mine';

  // Make an AJAX call
  this.http

    // Get an CSRF token
    .get(cookieUrl, options)

    // If getting the CSRF token fails, then submit
    // the login credentials to the server.
    .catch(() => this.http.put(sessionUrl, payload, options))
    .subscribe(

      // If everything goes ok, clear the error and do
      // something.
      () => {
        this.error = '';
        console.log('logged in'); // This is temporary
      },

      // An error occurred when the credentials were
      // PUT to the server.
      e => {

        // If the server responds with a 401, those
        // were bad credentials and you set the error
        // to an appropriate message.
        if (e.status === 401) {
          this.error = 'Could not log in with those credentials';
        }
      },
    );
}
```

You declare a new string variable named `error` after
the `username` and `password` variables so that your
new code will compile. You need a place to show the
error message, when it gets set, so you open the
component template and use the one-way data rendering
syntax you learned when you did your data binding
research.

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">

  <!-- Show an error when it occurs -->
  <div class="error">{{ error }}</div>

  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">
  <button class="cta"
          [disabled]="disableButton"
          (click)="submitCredentials()">Watch time</button>
</div>
```

After you refresh the page and type a bad username and
password, you see an error message that reads "Could
not log in with those credentials". When you type in
good credentials, like *maria* for both the username
and password, you see no error (it will disappear if
one was there, before) and you see a message in the
console that reads "logged in".

You will need to do something for real after a person
supplies a valid username and password. But, you take
a moment to bask in all of the information that you've
soaked in and used.

## What Did You Do?

In this section, you just created your first
interactive component! That's amazing! You look down
at your notes to review everything you did to finish
this component.

* Again, used the Angular CLI to generate a new
  component
* Used HTML and CSS to style the application and
  component
* Used `[(ngModel)]` two-way data binding to bind to
  text inputs
* Used `[disabled]` one-way data binding to set the
  value of the property on a button
* Used `(click)` to call a component method when
  a person clicks the button
* Used another of Angular's modules, `HttpModule` by
  including it in the application module
* Used type-based dependency injection to get an
  instance of the `Http` service into the login
  component
* Used Angular's AJAX service `Http` to make a GET
  request to get a CSRF token
* Used Angular's AJAX service `Http` to make a PUT
  request to the server to log in the person
* Displayed an error message by using Angular's
  one-way data rendering with `{{ variable_name }}`

You accomplished a lot! Most of the work from now on
will look like this, you realize: creating components
that make AJAX calls and binding values of those AJAX
calls to the HTML or into and out-of `<input>` and
other form fields. But, now that you've learned it
once, you know that you can do it, again, and even
better!

## The Final File Contents for the Component

`login-card.component.html`

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="error">{{ error }}</div>
  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">
  <button class="cta"
          [disabled]="disableButton"
          (click)="submitCredentials()">Watch time</button>
</div>
```

`login-card.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Http } from '@angular/http';

import 'rxjs/add/operator/catch';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  private username = '';
  private password = '';
  private error: string;

  constructor(private http: Http) { }

  ngOnInit() {
  }

  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

  submitCredentials() {
    const payload = {
      username: this.username,
      password: this.password
    };
    const options = {
      withCredentials: true
    };
    const cookieUrl = 'http://localhost:5000/api/clients';
    const sessionUrl = 'http://localhost:5000/api/session/mine';
    this.http
      .get(cookieUrl, options)
      .catch(() => this.http.put(sessionUrl, payload, options))
      .subscribe(
        () => {
          this.error = '';
          console.log('logged in');
        },
        e => {
          if (e.status === 401) {
            this.error = 'Could not log in with those credentials';
          }
        },
      );
  }

}
```

`login-card.component.css`

```css
:host {
  display: flex;
  flex-direction: column;
  width: 250px;
  align-self: center;
  padding: 0 0 8px 0;
  box-shadow: 0 2px 2px rgba(0, 0, 0, .24);
  background-color: white;
  margin-bottom: 8px;
}

.form-line {
  display: flex;
}

input {
  flex: 1 0 0px;
}

.figure {
  background-color: #b3e5fc;
}

.figure img {
  width: 100%;
}

.content {
  padding: 8px 16px;
}

.actions {
  padding: 8px 16px;
  display: flex;
  justify-content: flex-end;
}

.error {
  padding-top: 8px;
}

.error:empty {
  padding-top: 0;
}
```
