# Welcome to Single-Page Applications with Angular!

Most tutorials on learning to build single-page
applications (SPAs) and Angular start off with an
empty slate and introduce you to the different types of
functionality that Angular allows you to use to
create SPAs.

This unit takes you on a slightly different journey.
Instead of starting with a blank slate and building a
trivial UI-only application, we will start with a
fully-functioning Web application that uses no
JavaScript and use Angular to transition it into a
SPA one step at a time.

To get the most out of this unit, you should know HTML,
JavaScript, and **TypeScript**. You must know
**TypeScript**. Otherwise, a lot of what you see may
not make sense. This particular unit uses a Java-based
backend, so it'd be good to know that, too, but it's
not necessary since all you have to do is run it.

We'll start with a little history lesson to understand
how we got to where we are. Then, we'll dive into code
and start converting this multi-page application into
an Angular SPA one step at a time. We're going to learn

* how to get an Angular **application** onto a page;
* about **components**, the building blocks of Angular
  SPAs;
* **data binding** to tie together our HTML and
  JavaScript;
* how to **write services**, the way we should access
  data from our components;
* **routing** to give the illusion of multiple pages
  in our single-page app;
* how to do fancy HTML **rendering of lists** and
  stuff;
* learn how to get components to **communicate** with
  one another; and
* more!

So, fire up your command line and editor and let's get
going!

## A Little Note About Terminology

There are two radically different versions of the
framework that has the word "angular" in its name. The
old version, those in the 1.x series, are known as
*AngularJS*. The newer versions, 2.x and 4.x, those are
known as *Angular*. That is how this unit refers to
these ideas, so it'd make sense to put them in a table
so that it looks all official.

| Version | Name      | Website                                |
|---------|-----------|----------------------------------------|
| 1.X     | AngularJS | [angularjs.org](http://angularjs.org/) |
| 2.X     | Angular   | [angular.io](http://angular.io/)       |
| 4.X *   | Angular   | [angular.io](http://angular.io/)       |
\* The version in this unit.

## Where's Version 3?

The Angular team messed up the versioning of their
routing component which made version 3 unusable. So,
they skipped it and went to version 4.

## Software Used in This Unit

This unit refers to and uses the following software.
Please install it before continuing.

* [ng-cli](https://cli.angular.io/) - The Angular
  command line interface, used to generate stuff so you
  don't have to do it yourself.
* [Visual Studio Code](https://code.visualstudio.com/)
  \- Arguably, the best text editor to use when creating
  Angular applications.
