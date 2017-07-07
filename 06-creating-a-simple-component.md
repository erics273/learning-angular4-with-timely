# Your First Component: The "Not a user?" Box

The first component that you'll create is the
replacement for the "Not a user?" box on the login
screen.

![timely - the registration call-to-action box](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/sign-up-link-box-highlighted.png)

You recognize that this is a call-to-action for the
person visiting the page. You decide that you will
name this component the "Registration CTA" component.

From this, you can learn the bare minimum that it
takes to make an Angular component. There's nothing
fancy, here, no input fields or lists of things or
magical buttons. Just some text and a link. Nice and
easy, see?

## Creating the Component's Files

The Angular CLI does not just generate projects for
you, it also will generate components, too! To stick
with the Angular-recommended best practices, you
decide to use the CLI to create the component. You
open a command line and make sure you change the
working directory to `./timely-ui` and type:

```bash
npm run ng generate component registration-cta
```

The command tells you that it did some stuff by
creating a new subdirectory in `./src/app` and updated
the module file, too.

```bash
create src/app/registration-cta/registration-cta.component.css
create src/app/registration-cta/registration-cta.component.html
create src/app/registration-cta/registration-cta.component.spec.ts
create src/app/registration-cta/registration-cta.component.ts
update src/app/app.module.ts
```

*That's interesting that it updated the module*, you
think. Being the curious person you are, you open the
module file to see what happened in there.

## Inspecting the Update to the Module

Inside the `app.module.ts` file, you now see something
that looks like this.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

// This import is new!
import { RegistrationCtaComponent }
  from './registration-cta/registration-cta.component';

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent // This is new!
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

It seems that the CLI imported the new component into
the Angular module's file and added it to the
`declarations` property of the module. You realize
that if you had to add that yourself every time you
added a component, you would end up forgetting to do
this step quite often. It's nice that the Angular team
has your back and protects you from forgetting this
important step!

Now that the CLI has taken care of registering the
component for you, you can focus your time on the real
part of the problem: seeing and implementing the
component.

## Seeing the Component

Right now, your Angular application running at
[localhost:4200](http://localhost:4200) still shows
the generic placeholder content of a newly-generated
Angular application.

![timely - angular placeholder](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/placeholder-angular-content.png)

You decide to get rid of that junk and put your new
component there, instead. You open the `app.component.html`
file which currently contains

```html
<!--The whole content below can be removed with the new code.-->
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!!
  </h1>
  <img width="300" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxOS4xLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjxzdmcgdmVyc2lvbj0iMS4xIiBpZD0iTGF5ZXJfMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIgeD0iMHB4IiB5PSIwcHgiDQoJIHZpZXdCb3g9IjAgMCAyNTAgMjUwIiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCAyNTAgMjUwOyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8c3R5bGUgdHlwZT0idGV4dC9jc3MiPg0KCS5zdDB7ZmlsbDojREQwMDMxO30NCgkuc3Qxe2ZpbGw6I0MzMDAyRjt9DQoJLnN0MntmaWxsOiNGRkZGRkY7fQ0KPC9zdHlsZT4NCjxnPg0KCTxwb2x5Z29uIGNsYXNzPSJzdDAiIHBvaW50cz0iMTI1LDMwIDEyNSwzMCAxMjUsMzAgMzEuOSw2My4yIDQ2LjEsMTg2LjMgMTI1LDIzMCAxMjUsMjMwIDEyNSwyMzAgMjAzLjksMTg2LjMgMjE4LjEsNjMuMiAJIi8+DQoJPHBvbHlnb24gY2xhc3M9InN0MSIgcG9pbnRzPSIxMjUsMzAgMTI1LDUyLjIgMTI1LDUyLjEgMTI1LDE1My40IDEyNSwxNTMuNCAxMjUsMjMwIDEyNSwyMzAgMjAzLjksMTg2LjMgMjE4LjEsNjMuMiAxMjUsMzAgCSIvPg0KCTxwYXRoIGNsYXNzPSJzdDIiIGQ9Ik0xMjUsNTIuMUw2Ni44LDE4Mi42aDBoMjEuN2gwbDExLjctMjkuMmg0OS40bDExLjcsMjkuMmgwaDIxLjdoMEwxMjUsNTIuMUwxMjUsNTIuMUwxMjUsNTIuMUwxMjUsNTIuMQ0KCQlMMTI1LDUyLjF6IE0xNDIsMTM1LjRIMTA4bDE3LTQwLjlMMTQyLDEzNS40eiIvPg0KPC9nPg0KPC9zdmc+DQo=" />
</div>
<h2>Here are some links to help you start: </h2>
<ul>
  <li>
    <h2><a target="_blank" href="https://angular.io/tutorial">Tour of Heroes</a></h2>
  </li>
  <li>
    <h2><a target="_blank" href="https://github.com/angular/angular-cli/wiki">CLI Documentation</a></h2>
  </li>
  <li>
    <h2><a target="_blank" href="http://angularjs.blogspot.ca/">Angular blog</a></h2>
  </li>
</ul>
```

To know what you need to replace this with, you open
the `registration-cta.component.ts` file and look for
the `selector` property of the component.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({

  // The selector is what Angular uses to make
  // custom elements in the HTML!
  selector: 'app-registration-cta',

  templateUrl: './registration-cta.component.html',
  styleUrls: ['./registration-cta.component.css']
})
export class RegistrationCtaComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

In this case, you see the selector is
`app-registration-cta`. You delete all of the content
and replace it with your new component.

```html
<!-- Hello, my new component! -->
<app-registration-cta></app-registration-cta>
```

You save the file. The Angular process senses that
something has changed because you saved the file and
reload the browser to show you the new content!

![timely - registration-cta works](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/registration-cta-works.png)

[callout-info]
By default, when generating a component with the
Angular CLI, it will prefix your component with
"app-". This is to differentiate it from other kinds
of components.

If you plan on writing reusable components like fancy
buttons or layout widgets, you can change that prefix
using the command line parameter `--prefix` like

```bash
npm run ng -- generate component --prefix=my fancy-button
```

This will generate a component with the selector
`my-fancy-button` rather than `app-fancy-button`.
[/callout-info]

## Implementing the Sign-Up Box

You look at the files that the Angular CLI created
for you and pick up that there is now a file for each
kind of thing you need to write in the
`./timely-ui/src/app/registration-cta` directory.

| File Type | Purpose                                       |
|-----------|-----------------------------------------------|
| .html     | Hold the structure of the UI of the component |
| .ts       | Hold the behavior defined in TypeScript      |
| .css      | Hold the styles for the component             |
| .spec.ts  | Hold the tests for the component              |

Right now, this component does not have any behavior,
so you don't have anything to write in the `.ts` file
and nothing to test in the `.spec.ts` file. That
leaves the HTML and CSS to define.

### Specifying the HTML for the Component

From a structure standpoint, the HTML seems pretty
simple. There's a rectangle card thing with some
content that reads "Not a user?" and a button that
reads "SIGN UP!". So, you open the
`registration-cta.component.html` file and replace the
existing content with what you think you'll need.

```html
<div class="content">Not a user?</div>
<div class="actions">
  <button class="cta">Sign up!</button>
</div>
```

With the HTML in place, the browser now shows you the
strikingly unimpressive and unstyled content.

![timely - registration-cta unstyled](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/registration-cta-content-unstyled.png)

Now, it's time to style it.

### Specifying the CSS for the Component

You're using
[flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
in your CSS that you learned from
[Flexbox Froggy](http://flexboxfroggy.com/) a while
ago. You specified that the body should lay out with
flex column and you want to use that with the rest of
your application. You open the developer tools and see
the components laid out in the document tree (in the
green rectangle) and note that the `body` element is
indeed using flexbox (in the orange rectangle).

![timely - registration-cta layout](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/registration-cta-layout.png)

You note that between your `app-registration-cta`
component and the `body` is the `app-root` component.
You need to throw some flexbox onto that, too, so you
open the `app.component.css` file and put this CSS in
the empty file.

```css
:host {
  display: flex;
  flex-direction: column;
  flex: 1 0 0px;
}
```

This will stretch the `app-root` component to fill the
entire `body` (`flex: 1 0 0px`) and, then, provide a
vertically-oriented flexbox layout for all of the
components that you'll put inside the `app-root`.

With that done, you can now style the
`registration-cta` component. You open its CSS file
and type in

```css
:host {
  display: flex;
  flex-direction: column;
  width: 250px;
  align-self: center;
  padding: 8px 0;
  box-shadow: 0 2px 2px rgba(0, 0, 0, .24);
  background-color: white;
}

.content {
  padding: 8px 16px;
}

.actions {
  padding: 8px 16px;
  display: flex;
  justify-content: flex-end;
}
```

That almost does it. You realize that styling the
`<button>` element is a global thing. Also, you need
to gray up the background a little. You open the
file that holds the global styles, again, `styles.css`
and change it to change the `background-color` of the
`body` rule and add a new `button` rule.

```css
/* You can add global styles to this file, and also import other style files */
@import url('https://fonts.googleapis.com/css?family=Roboto');

html, body {
  height: 100%;
  padding: 0;
  margin: 0;
}

body {
  display: flex;
  flex-direction: column;
  align-items: stretch;

  /* A new background color for the app! */
  background-color: #f5f5f5;
}

body, input, textarea, select, button {
  font-family: 'Roboto', sans-serif;
}

body, button {
  font-size: 14px;
}

input, textarea, select {
  font-size: 16px;
}

h1, h2, h3, h4, h5, h6 {
  font-size: 1rem;
}

/* Make ALL of the buttons nice and flat. */
button {
  border: 0;
  background-color: transparent;
  text-transform: uppercase;
  color: #ff6d00;
}

/* Have the button change colors when pressed. */
button:active {
  background-color: #eeeeee;
}
```

Wow. You now have a component! It renders on the
screen and looks real pretty!

![timely - registration-cta styled](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/registration-cta-styled.png)

## What Did You Just Do?

In this lesson, you built your very first Angular
component. It didn't do much, but it shows you that
you can wrap up pieces of HTML, like the little
call-to-action card, and hide that behind an Angular
component so you can just use it like
`<app-registration-cta>` rather than having to mix up
HTML and CSS with other stuff. To do this, you

1. Used the Angular CLI to generate a new component;
1. Learned that the Angular CLI will import, declare,
   and register the new component for you in the
   `app.module.ts` file;
1. Used the new component in the `app-root` component
   with the specified `selector` of
   `app-registration-cta`;
1. Customized the appearance of the component by
   adding new HTML to the component's HTML file;
1. Customized the appearance of components by
   adding new CSS to the `app-root` *and*
   `app-registration-cta` components' CSS files; and,
1. Added global styling in the `styles.css`.

That was a lot of learning and just a little work to
have a really good-looking sign-up card on the Web
page.

You get up and reward yourself with a new cup of
«your favorite drink». Good job, you!

Not wanting to lose your momentum, you set your sights
on something more complicated.
