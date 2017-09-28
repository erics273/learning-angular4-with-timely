# The Client-Management List-Detail Page

Here's the stretch, the last and final page. You
know there's some difficulty in this one, but you're
not worried because you know you've got all the smarts
to get it done.

You pull up Michaela's app one more time and notice
that the client management stuff has three different
URLs:

* `/clients` - Lists the clients by themselves
* `/clients/new` - Shows the list of clients and the
  new client form
* `/clients/«clientId»` - Shows the list of clients
  and the client to edit

You realize that `/clients/new` and
`/clients/«clientId»` are just child paths of the
state that represents `/clients`. You know how to make
child paths! The only thing you need to figure out
is that `clientId` parameter thing and how to use it
with the Angular Router. You
keep a browser open to the [Routing & Navigation](https://angular.io/guide/router) to
keep all the weirdness close to you. The documentation
is kind of hard to read; you know that this is just a
pill you'll have to swallow. So you don't worry to much
about that.You're covered.

Otherwise, it's just data-binding and some `routerLink`s
and some `(click)` handling.

You know you can do this. You work slowly and
methodically, knocking out each piece of functionality
one-by-one, until the entire client management portion
of the application Just Works!

During the work, you refer back to the documentation
that you've come across and the code that you've
written. Luckily, you had it handy.

The rest you know, you've come across it many times,
like handling clicks and form submissions. You know
this may hurt a little as you do it, but you're so
close to the finish line, it'd make little sense to
give up, now...

You take a deep breath.

You lightly rest your fingertips on your keyboard.

Your eyes narrow a little.

You get to typing.
