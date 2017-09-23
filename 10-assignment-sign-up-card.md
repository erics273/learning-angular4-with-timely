# Convert the Sign-Up Card

You just created two Angular components and have all
the tools and knowledge you need to do this on your
own.

1. Generate a new component named "sign-up-card"
1. Delete the contents of `app.component.html` and replace
   them with `<app-sign-up-card></app-sign-up-card>` so
   you can see your new component as you develop it
1. Using similar HTML that you wrote for the login card,
   create the structure for the sign-up card and add an
   `<h1>` tag that has the content "Sign up!"
1. Using similar CSS that you wrote for the login card,
   style the sign-up card
1. Include the `Http` service in your components
   constructor
1. Declare `username` and `password` fields in your
   `SignUpCardComponent` class
1. Bind the username and password `<input>` fields in the
   HTML to their respective properties
1. Declare a getter method named `disableButton` and bind
   it to the `disabled` property of the button so that
   it's disabled until the person types content into both
   of the `<input>` fields
1. Declare a method named `submitSignup` and bind it to
   the click event of the button which will POST the
   username and password to
   `http://localhost:5000/api/users`
1. Handle successful registration by hiding any error
   message
1. Handle unsuccessful registration by showing an
   error message
