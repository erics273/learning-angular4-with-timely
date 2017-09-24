# Child Routes

While you were looking through the documentation for the
Angular Router, you noticed something that you thought
might help you with the routing of the real application
parts: child routes. Instead of having just one big
component controlling the whole screen, you read that you
can use a component to control the screen while its nested
(or child) components can control other parts of it.

You go back to the documentation and reread the
description of child routes.

> Child routes can be used to drill down from a more
> general feature to more specific one, or to
> implement a master/detail pattern. For instance, you
> might have a parent state called contacts which
> defines the contacts module, and renders a list of
> all the contacts.

While that makes sense, it's not totally clear what
the instructions are. You text your friend Alberto to
see if he know anything about this.

<style>
.callout-cool::before,
.callout-cool::after {
  background-image: none !important;
}
</style>
[callout-cool]
*You*: Hey, Alberto, I have a question.

*Alberto*: whats up?

*You*: I wanted to know about child routes in
Angular Router.

*Alberto*: yeah, no prob - like what?

*You*: Like how to use them?

*Alberto*: oh! ezpz! you create a nested route when
you define a children property in the route's declaration

*Alberto*: the router will put the child routes components
into a router-outlet in the template of the parent route

*You*: Ok, so if I understand this correctly...

*Alberto*: notime! archer just started! chat soon!
[/callout-cool]

You reread Alberto's messages and start nodding to
yourself. Right now you have a `main` route. If you
create child routes of that, say `main/entries`, then
that can be the route for the entries page. And, if
you create a route with the path `main/clients`, then that
can be the route for the clients list page. And, if
you create a state named `main/report`, then that
could be the route for the report page! Finally, you'll
want just '/main' to redirect to '/main/entries' because
that's the default view for the application!

You're so excited you can barely contain yourself! You set
to creating those three new components by running the
comopnent generate command for each of `time-entries`,
`clients-list`, and `report`.

```bash
npm run ng generate component time-entries
npm run ng generate component clients-list
npm run ng generate component report
```


Then, you make some new routes in the `app.module.ts`
file.

```typescript
// imports above...

const appRoutes: Routes = [
  { path: 'login',  component: LoginPageComponent },
  { path: 'signup', component: SignUpCardComponent },
  {
    path: 'main',
    component: MainScreenComponent,
    canActivate: [LoggedInGuard],
    children: [
      { path: 'clients', component: ClientsListComponent },
      { path: 'entries', component: TimeEntriesComponent },
      { path: 'report', component: ReportComponent },
      { path: '', redirectTo: 'entries', pathMatch: 'full' }
    ]
  },
  { path: '',  redirectTo: '/login', pathMatch: 'full' }
];

// @NgModule below...
```

Looking back at what Alberto wrote to you, you see that
these nested routes will show up in a `<router-outlet>`
element in the parent route. The parent route for these
three new nested states is the `main` state; you go add a
`<ui-view>` viewport to the `main-screen.component.html`
file.

```html
<header class="app-bar">
  <h1 class="app-bar-title">Timely</h1>
  <div class="app-bar-spacer"></div>
  <button class="button" ng-click="mainScreen.logout()">Sign out</button>
</header>
<nav class="tabs">
  <ul class="tabs-list">
    <li class="tabs-item tabs-item-selected"><a href="/">Today's Work</a></li>
    <li class="tabs-item"><a href="/clients">Clients</a></li>
    <li class="tabs-item"><a href="/report">Report</a></li>
  </ul>
</nav>

<!-- A new way to see the world! -->
<router-outlet></router-outlet>
```

## Time to Fix the Tabs

Now, you want the tabs to work correctly. Right now, they
cause Angular to break because they go to paths that
aren't defined in the route. You should change them to
`routerLink` directives. You go into
`main-screen.component.html` and make the changes.

```html
<header class="app-bar">
  <h1 class="app-bar-title">Timely</h1>
  <div class="app-bar-spacer"></div>
  <button class="button" (click)="logout()">Logout</button>
</header>
<nav class="tabs">
  <ul class="tabs-list">

    <!--- NOW WITH LOVELY ROUTER LINKS! -->
    <li class="tabs-item"><a routerLink="/main/entries">Today's Work</a></li>
    <li class="tabs-item"><a routerLink="/main/clients">Clients</a></li>
    <li class="tabs-item"><a routerLink="/main/report">Report</a></li>

  </ul>
</nav>
<router-outlet></router-outlet>
```

The tabs now work correctly when you click on them. What
does not work is the selected tab. You head over to the
[documentation](https://angular.io/guide/router#summary)
again, and find the answer in the summary. The Angular
Router provides a `routerLinkActive` directive, the value
of which gets applied as a CSS class name to the element
that it's on for the `<a>` tag that has the `routerLink`.
You change the `main-screen.component.ts` file to use the
`routerLinkActive`.

```html
<header class="app-bar">
  <h1 class="app-bar-title">Timely</h1>
  <div class="app-bar-spacer"></div>
  <button class="button" (click)="logout()">Logout</button>
</header>
<nav class="tabs">
  <ul class="tabs-list">
    <li class="tabs-item" routerLinkActive="tabs-item-selected"><a routerLink="/main/entries">Today's Work</a></li>
    <li class="tabs-item" routerLinkActive="tabs-item-selected"><a routerLink="/main/clients">Clients</a></li>
    <li class="tabs-item" routerLinkActive="tabs-item-selected"><a routerLink="/main/report">Report</a></li>
  </ul>
</nav>
<router-outlet></router-outlet>
```

You lean back in your Herman Miller and stretch your
arms and feel the glow of a job well done spread
across you. You whisper to yourself, "You did good,
kid."

## What Did You Do?

With the help of your friend Alberto and the Angular
Router documentation, you discovered how to use the child
routes feature. From reading posts on Stack Overflow, you
know this is no small achievement! You learned how to

* declare child routes;
* transition to child routes;
* use a `<router-outlet>` in the parent route to show the
  child states; and,
* apply the `routerLinkActive` directive to change the
  selected state of an HTML element.

Now, you want to get the application working by
showing data. To get the data, you know that you
should create services, so that's what you've decided
to do next.
