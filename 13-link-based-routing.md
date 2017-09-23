# Routing: From Two Pages to One Page

In your application, right now, you have two separate
components, the Login Page component and the Sign Up Card
component. That's nice, and all, but you *want* (no
**need**!) a single-page application. You strap yourself
into your Herman Miller and fire up the ol' editor and
browser to implement **routing**.

[callout-info]
**Routing** in a single-page Web application means
changing the entire view (or parts of the view) in
response to some event like a person clicking on a
button *without* refreshing the page from the server.
[/callout-info]

## Importing the Routing Module

You want to use this idea of routing to make the screen
change (and, hopefully the URL, too!) when users do stuff.
You search for "angular routing docs" over at
[duckduckgo](https://duckduckgo.com/?q=angular+routing+docs)
and read the [Angular Routing &
Navigation](https://angular.io/guide/router) guide. That
gives you more than enough information to get this simple
swap out that you want for right now.

## Planning

You look at the UI and determine that when a person clicks
the "SIGN UP!" link that it should replace the
`app-login-page` component with the `app-sign-up-card`
component, a one-for-one switch.

![timely - routing between two components](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/simple-routing-design.png)

## Importing the Angular Router

As you read in the guide, you have to `import` the actual
TypeScript module that contains the router. So, you
change your `app.module.ts` file to look like this.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

// New import for the router! YAY!
import { RouterModule, Routes } from '@angular/router';

import { AppComponent } from './app.component';
import { RegistrationCtaComponent } from './registration-cta/registration-cta.component';
import { LoginCardComponent } from './login-card/login-card.component';
import { SignUpCardComponent } from './sign-up-card/sign-up-card.component';
import { LoginPageComponent } from './login-page/login-page.component';

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent,
    LoginCardComponent,
    SignUpCardComponent,
    LoginPageComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

That takes care of getting the `RouterModule` and the idea
of `Route`s imported. Now, you figure you need to
configure the router.

## Configuring the Angular Router

Again, from the guide, you see that you want to define
some routes for the Login Page component and the Sign Up
Card component. Each of those routes will have

* the URL **path** that should map to the component; and,
* the **component** that the router should render.

You also want to set up a default route for when someone
just types in the default URL, `http://localhost:4200`
while you're developing, who knows what once you hand this
over to Monica.... So, to do that, you'll need

* the **default path** which will just be `'/'`;
* the path to redirect to, which should be the path for
  the login; and,
* some value to specify that you want this to only happen
  on a full match.

From the documentation, it looks like you just define an
array of routes and, then, use the `RouterModule` to bake
them into the `imports` of the Angular `@NgModule`. You
scaffold that out and, now, your `app.module.ts` file
looks like this:

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

// Define some new routes!
const appRoutes: Routes = [
  // My routes will go here
];

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent,
    LoginCardComponent,
    SignUpCardComponent,
    LoginPageComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,

    // Import the routes with RouterModule
    RouterModule.forRoot(appRoutes)

  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

From your design, you identify two states, the log-in
state and the sign-up state. You define a couple of states
and add them to the array of `Route` objects with the
default route redirect, too.

```typescript
const appRoutes: Routes = [
  // NEW ROUTES FOR TIMELY!
  // Show the login page for the path /login
  { path: 'login',  component: LoginPageComponent },

  // Show the sign-up card for the path /signup
  { path: 'signup', component: SignUpCardComponent },

  // Redirect to login for empty path
  { path: '',  redirectTo: '/login', pathMatch: 'full' }
];
```

Now that you have the states configured, you can use
the router to transition between the states rather
than navigating to another page.

## Showing the Current State

Right now, the `app.component.html` file still has the
content of `<app-login-page></app-login-page>`. The
routing guide says that you need to replace that with what
it calls a *router outlet* So, you replace the entire
contents of `app.component.html` with a `router-outlet`
tag so that it looks like this.

```html
<!-- Hello, Routersville!  -->
<router-outlet></router-outlet>
```

You refresh the page and you still see the same Login
Page. But, you look at the URL and see that it has now
redirected you to http://localhost:4200/login which is
pretty cool.

You now type http://localhost:4200/signup into the
address bar of the browser and see your Sign Up Card! This
router thing is working!

You go back to http://localhost:4200 (which redirects you)
and wonder about how to get that "Sign up!" button to get
you over to the Sign Up Card. The first thing in the
documentation talks about using **router links**. But, you
have a button. You think about the problem for a minute
and decide that the button doesn't really have to be a
button! It's already got this specific kind of style that
doesn't look button-y in the traditional sense. You could
style an `<a>` tag with a "button" class to look just
like, too!

## Transitioning Using a Link

You see the "SIGN UP!" button in the `registration-cta`
component and open the `registration-cta.component.html`
to re-familiarize yourself with it.

```html
<div class="content">Not a user?</div>
<div class="actions">
  <button class="cta">Sign up!</button>
</div>
```

You change that so that you have a link rather than a
button on the page.

```html
<div class="content">Not a user?</div>
<div class="actions">

  <!-- Good-bye button, hello link -->
  <a class="button cta">Sign up!</a>

</div>
```

That messed up the way it looks, so you go to the global
stylesheet file, `styles.css`, and find everywhere you
have a `button` mentioned and add the selector `a.button`
to it, too. Then, because you know that `<a>` tags don't
have any padding, you specify padding for all button-y
looking things. You also remove the underline for button-y
anchor tags.

```css
a.button, /* Hello, a tag */
button {
  border: 0;
  background-color: transparent;
  text-transform: uppercase;
  color: #ff6d00;
  padding: 4px 8px;  /* Padding for all button-y things */
  text-decoration: none; /* No underlines for <a> tags */
}

a.button:hover, /* Hello, a tag */
button:active {
  background-color: #eeeeee;
}

a.button[disabled], /* Hello, a tag */
button[disabled] {
  color: #eeeeee;
}
```

The "Sign up!" link doesn't do anything, yet. The
documentation states that instead of using an `href`
attribute, that you should use the `routerLink` directive
to specify the path you want. On the "Sign up!" link, you
add that.

```html
<div class="content">Not a user?</div>
<div class="actions">

  <!-- routerLink FTW! -->
  <a routerLink="/signup" class="button cta">Sign up!</a>

</div>
```

With that changed, when you click the "SIGN UP!"
button, it successfully transitions to the new state
and changes the URL from http://localhost:4200/login to
http://localhost:4200/signup!

And, now, you fix a UI bug that's been bothering you
since you started looking at this application.

> When you're on the sign-up card, there's no way
> to go back to the login page!

All you do is add an `<a>` tag before the "Register!"
button with the correct `routerLink` back to the login
state.

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="error">{{ error }}</div>
  <h1 class="header">Sign up!</h1>
  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">

  <!-- Who says you can never go home, again? -->
  <a routerLink="/login" class="button">Cancel</a>

  <button class="cta"
          [disabled]="disableButton"
          (click)="submitSignup()">Register!</button>
</div>
```

## What Did You Do?

While you know that this has only scraped the surface of
routing with the Angular router, you sure do feel like you
learned a lot along the way! You learned to

* import another Angular module into your module;
* plan the transitions between states of your
  application;
* configure the RouterModule with states that describe
  your application;
* configure a default route; and,
* use `routerLink` on `<a>` tags to initiate transitions
  between different UI states.
