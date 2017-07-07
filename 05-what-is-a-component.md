# What Does Angular Mean by "Component"?

The answer to that is fairly simple:

[callout-info]
Components are the most basic building block of an UI
in an Angular application. An Angular application is a
tree of Angular components. Angular components are a
subset of directives. Unlike directives, components
always have a template and only one component can be
instantiated per an element in a template.
[/callout-info]

In practice, what that means is, instead of writing
some HTML like this and including it everywhere you
want to use it

```html
<div class="star-rating">
  <div class="star-rating-1">★</div>
  <div class="star-rating-2">★</div>
  <div class="star-rating-3">★</div>
  <div class="star-rating-4">★</div>
  <div class="star-rating-5">★</div>
</div>
```

in addition to some JavaScript using jQuery in some
file like `site.js` where, somewhere in there, you
might be able to find

```javascript
// DO NOT DELETE! THIS HANDLES RATINGS!
$('.star-rating').click(function (e) {
  var $target = $(e.target),
      $this = $(this),
      ratingName = e.target.attr('class'),
      rating = parseInt(ratingName.substring(12));
  for (var i = rating; i <= 5; i += 1) {
    $this.children('.star-rating-' + i)
         .addClass('selected');
  }
});
```

This is hard to maintain. Unless you get smart, you
may end up with a lot of places that have the
`<div class="star-rating">` block. And, because the
JavaScript is in some file that contains a lot of
other JavaScript that doesn't have anything to do with
star-based ratings, figuring out where to go to fix
a bug and making sure that it gets fixed in all the
places can be really, really hard.

So, Angular (and frameworks like Angular) say to you,
"Hey, you want a little widget that represents a
five-star rating feature? Let me help you with that!"
In Angular, you just declare a component that would
show the same HTML and some JavaScript in the
component file, and you get to then write this HTML.

```html
<star-rating></star-rating>
```

everywhere you want to see five stars instead of the
monster HTML block you wrote before.

## Once Again

A component is a bundle of HTML, CSS and JavaScript
that Angular will put into a Web page for you and
manage as if they're all one thing. This means that
you can have things that look like your own HTML tags
in your Web page that could make your Web page source
look like this, though I can't imagine what you'd try
to accomplish with such a thing.

```html
<body>
  <dirty-socks></dirty-socks>
  <pizza-slice></pizza-slice>
  <broken-cellphone></broken-cellphone>
</body>
```

(Although, now that I see that, it could be a Web
application that describes my college dorm room.)

Now, you should go and make a simple component that
takes you one step closer to making Timely!
