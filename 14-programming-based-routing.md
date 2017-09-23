# Routing: Changing State on Login or Registration

You're coming back to your computer feeling pretty
good after having a little demo with Michaela showing
her the work that you've accomplished so far. Michaela
said something like, "That would've taken me a week!"
You thought, "Good job, me. Good job."

You sit down and run through the demo, again, checking
out the components that you've written for getting
access to Timely. You realize that it's time to take
the big jump and get into the real application itself.

Now, you want to tackle the ability to get past the
login and registration screens into the application
using AJAX and the SPA rather than having the browser
navigate away.

You decide to take a baby step and just get the
routing figured out before starting on adapting the
time entry, client, and reporting screens.

## Creating a Placeholder Component

You choose to create a component that will,
eventually, become the main application. But, for now
at least, it'll just be something to show that your
code is set up right by displaying a "SIGN OUT"
button that you can use to get back to the log-in
screen.

```bash
npm run ng generate component main-screen
```

Inside the `main-screen.component.html` file, you
place a single button bound to a method in the
controller.

```html
<button (click)="logout()">Logout</button>
```

[callout-info]
When you just want to initiate an action on the
controller because someone clicked something in your
component, use the `click` Angular directive surrounded
by parentheses.
[/callout-info]

You peek in the `SessionApiController`, again, to see how
Michaela wrote the server-side code and it looks like you
need to send a `DELETE` HTTP call to `/api/session/mine`.
Luckily, the `Http` service has the convenience method on
it!

```typescript
import { Component, OnInit } from '@angular/core';

// Import the Http service for AJAX calls
import { Http } from '@angular/http';

@Component({
  selector: 'app-main-screen',
  templateUrl: './main-screen.component.html',
  styleUrls: ['./main-screen.component.css']
})
export class MainScreenComponent implements OnInit {

  // Add a constructor member variable declaration
  constructor(private http: Http) { }

  ngOnInit() {
  }

  logout() {
    const options = {
      withCredentials: true
    };
    const logoutUrl = 'http://localhost:5000/api/session/mine';
    this.http
      .delete(logoutUrl, options)
      .subscribe(

        // On success, log a message for now?
        () => console.log('logged out'),

        // On error, log a message for now?
        error => console.error('Error logging out', error)
      );
  }

}
```

## Adding the New State

You open `app.module.ts` and define and register
another state to the configuration.

```typescript
// imports up here

const appRoutes: Routes = [
  { path: 'login',  component: LoginPageComponent },
  { path: 'signup', component: SignUpCardComponent },

  // This is the main application!
  { path: 'main',   component: MainScreenComponent },

  { path: '',  redirectTo: '/login', pathMatch: 'full' }
];

// @NgModule down here
```

## Responding to a Successful Login

You review the `login-card` and `sign-up-card` components
to remind yourself how they currently work. They both use
the `routerLink` directive from Angular to navigate the
browser to a new page. In this case, the person interested
in logging in isn't just clicking a link; no, that person
is clicking a button and, depending on the outcome, then
the application should transition.

You look over the documentation for the Angular Router and
find the `navigate` method of the `Router` object down in
the [Navigating back to the list
component](https://angular.io/guide/router#navigating-back-to-the-list-component)
section. You add that functionality to your
`login-card.component.ts` file by getting a `Router`
object injected and using it in the `submitCredentials()`
method.

```typescript
import { Component, OnInit } from '@angular/core';
import { Http } from '@angular/http';

// IMPORT THE ROUTER FOR ROUTING GOODNESS
import { Router } from '@angular/router';

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

  // GET A ROUTER OBJECT FOR NAVIGATING AFTER AUTHENTICATION
  constructor(private http: Http, private router: Router) { }

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

          // NAVIGATE THE APPLICATION TO THE /main STATE
          this.router.navigate(['/main']);
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

You make similar changes in the
`sign-up-card.component.ts` file, too, adding the `import`
statement, constructor-member declaration, and call to
`this.router.navigate`.

You run it and, sure enough, everything works! No more
browser redirection! When you log in, you see one
button that, when you click it, it returns you to the
log in page!

## Fixing the `main-screen` Component

Now that you know how to transition using the `navigate`
method of the `Router`, you go back to the
`main-screen.component.ts` file and put a redirect to
`'/'` after logging out.

```typescript
import { Component, OnInit } from '@angular/core';
import { Http } from '@angular/http';
import { Router } from '@angular/router';

@Component({
  selector: 'app-main-screen',
  templateUrl: './main-screen.component.html',
  styleUrls: ['./main-screen.component.css']
})
export class MainScreenComponent implements OnInit {

  constructor(private http: Http, private router: Router) { }

  ngOnInit() {
  }

  logout() {
    const options = {
      withCredentials: true
    };
    const logoutUrl = 'http://localhost:5000/api/session/mine';
    this.http
      .delete(logoutUrl, options)
      .subscribe(

        // GO BACK TO THE LOGIN PAGE
        () => this.router.navigate(['/']),

        error => console.error('Error logging out', error)
      );
  }

}
```

## Guarding the Main Application

You feel pretty good about this setup, but something is
nagging you. You make sure you're logged out and type
http://localhost:4200/main into the address bar and see
the main page. That's not what you want. You want that
locked down. You go back to the Routing & Navigation guide
and find the section [Guard the admin
feature](https://angular.io/guide/router#guard-the-admin-feature).
You want a *guard* that responds to inquiries by Angular
as to whether or not a person is allowed to load the
component at the specified path. Luckily, the Angular CLI
has a way to generate guards for us.

```bash
npm run ng generate guard logged-in

> ng "generate" "guard" "logged-in"

  create src/app/logged-in.guard.spec.ts (371 bytes)
  create src/app/logged-in.guard.ts (405 bytes)
```

You open this guard file and see the following.

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class LoggedInGuard implements CanActivate {
  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    return true;
  }
}
```

You know that you're going to want to navigate when the
person trying to get to the main page is not logged in, so
you create a constructor, too, that has a `Router`
injected into it. You also want to use `Http` to find out
if the person is currently logged in. So, you add that to
the constructor, too.

In the `canActivate` method, you make a call to the URL
http://localhost:5000/api/clients which should cause an
error if the person trying to access the resource is not
logged in. You return that use of `Http` so that the
guard can do its duty. You look at your file which now
has this code in it.

```typescript
import { Injectable } from '@angular/core';

// GET THE ROUTER SO THE GUARD CAN NAVIGATE AWAY
import { Router, CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';

import { Observable } from 'rxjs/Observable';

// GET HTTP BECAUSE AJAX
import { Http } from '@angular/http';

// ADD SOME OBSERVABLE OPERATORS FOR THE HANDLING OF ERRORS
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch';
import 'rxjs/add/observable/of';

@Injectable()
export class LoggedInGuard implements CanActivate {

  // GET THE ROUTER AND HTTP OBJECTS FROM ANGULAR
  constructor(private router: Router, private http: Http) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {

    // DECLARE SOME HANDY CONSTANTS
    const checkUrl = 'http://localhost:5000/api/clients';
    const options = {
      withCredentials: true
    };

    // MAKE AN HTTP GET CALL AND, IF IT SUCCEEDS, RETURN TRUE
    // OTHERWISE, NAVIGATE AWAY AND RETURN FALSE
    return this.http
      .get(checkUrl, options)
      .map(() => true)
      .catch((error, caught) => {
        this.router.navigate(['/login']);
        return Observable.of(false);
      });
  }

}
```

You open up the `app.module.ts` file to add the guard to
the route for '/main' as well as adding it to the
`providers` array in the `@NgModule` because the Routing &
Navigation guide instructed you to do that.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { RouterModule, Routes } from '@angular/router';

import { AppComponent } from './app.component';
import { RegistrationCtaComponent } from './registration-cta/registration-cta.component';
import { LoginCardComponent } from './login-card/login-card.component';
import { SignUpCardComponent } from './sign-up-card/sign-up-card.component';
import { LoginPageComponent } from './login-page/login-page.component';
import { MainScreenComponent } from './main-screen/main-screen.component';

// IMPORT THE NEWLY CREATED GUARD. ANGULAR DOES NOT DO THIS
// FOR YOU AUTOMATICALLY.
import { LoggedInGuard } from './logged-in.guard';

const appRoutes: Routes = [
  { path: 'login',  component: LoginPageComponent },
  { path: 'signup', component: SignUpCardComponent },

  // GUARD THE MAIN SCREEN WITH THE CANACTIVATE CALL
  { path: 'main',   component: MainScreenComponent, canActivate: [LoggedInGuard] },

  { path: '',  redirectTo: '/login', pathMatch: 'full' }
];

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent,
    LoginCardComponent,
    SignUpCardComponent,
    LoginPageComponent,
    MainScreenComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(appRoutes)
  ],

  // ADD THE GUARD TO THE PROVIDERS LIST
  providers: [LoggedInGuard],

  bootstrap: [AppComponent]
})
export class AppModule { }
```

You save everything and, lo and behold, you cannot get to
'/main' until after you log into the application! You do
a little victory dance in your chair and decide it's time
for some caffeine.

## What Did You Do?

You discovered a whole bunch of routing stuff. Boiling it
down to an enumerated list, you learned:

* how to navigate based on conditional logic using the
  `navigate` method of the `Router` object;
* what a guard was and how to implement one to prevent
  someone from seeing the main screen before logging in;
  and,
* how to register the guard with a `Route` and the app
  module.

You can sense that it will be pretty smooth sailing
from here. All you have to do is upgrade the rest of
the application.

How hard can that be?
