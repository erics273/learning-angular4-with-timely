# Getting Started: Create an Angular Application

You hate starting a new project, setting up files and
directories and configuration and build scripts, it's
just so tedious! You scowl at the screen for a moment.
Then, your eyes widen and the scowl disappears,
replaced by a sly smile. You remember that you
installed the Angular command line interface (CLI)! It
does all that boring work for you!

You quickly open a command prompt, change the working
directory to one of your development directories, make
sure you have a good connection to the Internet, and
type

```bash
ng new timely-ui
```

and the CLI installs a whole slew of stuff for you in
a new `timely-ui` directory that it created. When it
finishes, you use Visual Studio Code to open the
directory by typing

```bash
cd timely-ui
code .
```

and the editor opens showing all of the files that the
Angular CLI created on your behalf.

You go back to the command line and type

```bash
npm start
```

to start the new application, open your browser,
navigate to
[`http://localhost:4200`](http://localhost:4200), and
see the following placeholder content.

![timely - angular placeholder](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/placeholder-angular-content.png)

*Well, that's boring*, you think. You go back to your
text editor and look at all the files that it created
for you.

## Anatomy of an CLI-Generated Angular Application

You decide to peruse through the directories that
the Angular CLI created and figure out what all just
appeared.

### The Top-Level Directory: `./timely-ui`

In the top level of the applications directory, you
see these files. You can figure out what they do from
their names and what you see in them.

* `README.md` - Just the read-me file for Angular.
  Boring.
* `karma.conf.js` - You remember from another project
  that `karma` is a test runner for JavaScript, so it
  makes sense that Angular, with its emphasis on
  testing, has some test runner built into the
  skeleton project. You have the inkling that you will
  write some tests before you're done.
* `package-lock.json` - NPM package junk.
* `package.json` - NPM package junk **AND** the
  scripts that the Angular team has created for you to
  run stuff. You see six commands in the `"scripts"`
  portion of the configuration:
  * `ng` - This looks to run the Angular CLI for you.
  * `start` - This runs the app, as you've seen
    already.
  * `build` - You guess that this builds the app for
    production.
  * `test` - This probably runs the *karma* tests.
  * `lint` - This probably runs the TypeScript linter.
  * `e2e` - You guess this runs the end-to-end tests.
* `protractor.conf.js` - You
  [duckduckgo](http://duckduckgo.com/) search this and
  find out that it's an end-to-end testing framework
  for Angular. You might end up writing some of that,
  too.
* `tsconfig.json` - You see the "ts" prefix on there
  and figure that this is the configuration file for
  the TypeScript compiler, TypeScript being the
  default language that Angular uses for its code.
* `tslint.json` - Again, with the "ts" prefix, you
  know that this must do with TypeScript. From your
  past, you know that a *linter* checks code to make
  sure that it conforms to best practices. You guess
  that the Angular team has some settings that they
  think are pretty good, so you decide to go with the
  flow.

### Inside the `./timely-ui/src` Directory

Inside the `src` directory is where you know the
action is. Well, not a lot of action, really. That's
probably in another directory because, what you see in
this one, are these files.

* `favicon.ico` - The abomination knows as the
  [favicon](https://en.wikipedia.org/wiki/Favicon).
* `index.html` - This page seems to be the page that
  should be served for the application. In it,
  everything seems pretty standard except for that
  `<app-root>` element. You guess that probably the
  way Angular gets its stuff into the Web page.
* `main.ts` - This file contains the TypeScript that
  seems to bootstrap, or start, the application. You
  guess that from the call to the `bootstrapModule`
  method.
* `polyfills.ts` - This seems to be some file that
  contains all of the
  [polyfill](https://en.wikipedia.org/wiki/Polyfill)s
  that Angular wants to run as well as some options
  for you to include, if you want.
* `styles.css` - Global styles. Good to keep in mind.
* `test.ts` - Like `main.ts` is the main file for the
  Web app, this seems to be the main file for unit
  tests.
* `tsconfig.app.json` - More configuration, this one
  for the "app", it seems.
* `tsconfig.spec.json` - More configuration, this one
  for "spec"s? You shrug your shoulders at "spec" and
  decide to figure it out later.
* `typings.d.ts` - You know that this is some weird
  TypeScript magic that is better left alone.

### Inside the `./timely-ui/src/app` Directory

"Finally," you mutter under your breath, "some real
Angular stuff!" These five files appear to you to be
the application that Angular was showing you in the
browser that had the big "A" icon and the links to
the documentation stuff. You pick through these,
one-by-one because they seem the most interesting to
you.

#### The `app.module.ts` File

You remember from the `main.ts` file that call to the
`bootstrapModule` method and figure that this "module"
is a good place to start. You open the file and see

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

That all looks like pretty normal TypeScript to you.
You really only wonder about the Angular-provided
`import`s that you see. Most importantly, what is that
`@NgModule` decorator? A little searching finds this
statement in the Angular documentation:

[callout-info]
**NgModules** help organize an application into
cohesive blocks of functionality.
[/callout-info]

OK. So, this so-called "app module" must contain all
of the functionality of the app. That makes sense.

It's importing the `AppComponent`, so that's the next
file you open.

#### The `app.component.ts` File

This is what you expect some Angular magic to look
like. You look over the file, soaking in the details.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';
}
```

That's what you wanted to see. So, this is a
"component". You know that Angular is all about these
things. You know that you'll spend the majority of
your time writing these things, so you look it over
pretty hard because you're going to research this
stuff, next.

This file references the `html` and `css` files, so
you look at those, next.

#### The `app.component.html` and `app.component.css` Files

The CSS file is empty. You make the guess that this
component has no styling. You look in the browser,
again, and recognize that, yes, this has *no styling*.

![timely - angular placeholder](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/placeholder-angular-content.png)

In the HTML file, you see the stuff that obviously
appears in the browser, links and image and ... oh,
wait! You notice some Angular magic, already. At the
beginning of the HTML file, you see

```handlebars
<!--The whole content below can be removed with the new code.-->
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!!
  </h1>
```

You look in your browser and see that it shows
"Welcome to app". Angular must be replacing the
`{{title}}` with the word "app". *Where's that coming
from*, you ask yourself. Then, you realize that, in
the `app.component.ts` file, you saw, at the end of
the file

```typescript
export class AppComponent {
  title = 'app';
}
```

Obviously, that instance variable on an instance of
the `AppComponent` class is feeding that value into
the template. You wonder what would happen if you
change it...

#### The `app.component.spec.ts` File

This is the only file that hasn't been referenced in
this section of code, so far. But, there's that "spec"
thing, again. Maybe you'll get your answer about what
that means. You open the file and see

```typescript
import { TestBed, async } from '@angular/core/testing';

import { AppComponent } from './app.component';

describe('AppComponent', () => {
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [
        AppComponent
      ],
    }).compileComponents();
  }));

  it('should create the app', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app).toBeTruthy();
  }));

  it(`should have as title 'app'`, async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app.title).toEqual('app');
  }));

  it('should render title in a h1 tag', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    fixture.detectChanges();
    const compiled = fixture.debugElement.nativeElement;
    expect(compiled.querySelector('h1').textContent).toContain('Welcome to app!!');
  }));
});
```

Oh! You immediately recognize this as a `karma` test!
This file must contain tests for the `AppComponent`
class/component! Open question answered!

### Inside the Other Directories

You see some other directories and figure those out
with intuition and some good old Internet searching.

* `./timely-ui/e2e` - This is where Angular requires
  you to put end-to-end tests, away from the main
  application.
* `./timely-ui/src/assets` - This is where the Angular
  server will send static content like images and
  stuff. Cool.
* `./timely-ui/src/environments` - This seems to
  contain different files for different environments
  that the application will build in, production or
  non-production. You don't know of any differences
  that you'll need, yet, so you'll just leave those
  be, for now.

## What Did You Do?

The Angular CLI did a lot for you, creating an entire
application structure *and* tests for you. Thank you,
again, Angular CLI! You saved me a bunch of typing!

In review, you

* Used the Angular CLI to generate a new Angular
  project;
* Discovered all of the different commands that you
  have available to run the Angular app in different
  ways;
* Figured out where the different configuration files
  for all of the tools lived;
* Determined where, in an Angular app, to put static
  content like images;
* Got your first look at Angular modules and
  components; and,
* Previewed what tests would look like.

All of that at the click of some keys and a mouse.
This is so much more delightful than the old school
way of having to generate all of this stuff yourself.
The Angular team really did a nice job at making your
life easier in getting started.

So awesome!

Now, off to Componentsville!

## BONUS! Get Some Styling!

As a Web developer, there are always some basic CSS
that you want to get into your applications regarding
fonts and padding and stuff. These are normally global
settings, so you decide to take care of that, now. You
open the `./timely-ui/src/styles.css` file and type in
the following.

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
```

That sets the proper font size and family to
[Roboto](https://fonts.google.com/specimen/Roboto) and
removes padding and margin from the document around
the edges. It also makes all the headings the same
size so that you can use the meaningful `<h1>` tag
without it dominating the layout. You can style that
back in, if you need to. That almost always makes
stuff look better, you feel, and gives you the best
control over your page.
