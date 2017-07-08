# Your Second Component (Part 1 of 3): Get It On The Page

You did such a great job with the last component, that
you don't want to lose your momentum. You choose to
take the next step and try out something harder. You
choose to create the login card with an Angular
component.

![timely - the login box](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/login-box-highlighted.png)

You immediately realize that this one is more
difficult than the rest. This has some behavior, so
you make a little list for yourself:

> The login component should:
>
> 1. Enable the "WATCH TIME" button only when both
> text fields have input
> 1. Use AJAX to authenticate the user

You follow the five steps of general creation.

## Generate and Get the Component Looking Good

### Step 1: Create the files

You decide to name your component `login-card` and
run the generator to make it.

```bash
npm run ng generate component login-card
```

### Step 2: Add the component file to the Web page

You want to add the component above the registration
call-to-action box like you saw in Michaela's version
of Timely. You open the `app.component.html` and add
the tag into there.

```html
<!-- Hello, my new component! -->
<app-login-card></app-login-card>
<app-registration-cta></app-registration-cta>
```

![timely - the new login component](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/login-card-new-on-page.png)

### Step 3: Get the Image

There's Michaela's clock logo that she made in her
application. You go into her code and find the clock
logo at `«java project»/src/main/resources/static/img/clock.png`
under the code of her project and copy it to the
`./timely-ui/src/assets` directory of yours.

### Step 4: Add the HTML into the component HTML file

You refer back to the UI of the original application
and see the image, a text `<input>`, a password
`<input>`, and a button, all inside another rectangle
with a shadow. You know how to do that. You open the
`login-card.component.html` file and replace the
"login-card works!" HTML with your own.

```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="form-line">
    <input type="text" placeholder="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password">
  </div>
</div>
<div class="actions">
  <button class="cta">Watch time</button>
</div>
```

That definitely gets the structure in there, but you
can't help but laugh out loud when you see what
appears in the browser after you save it.

![timely - unstyled login card](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/login-card-no-style.png)

### Step 5: Style the `login-card` Component

In the `login-card.component.css` file, you add some
CSS to make the login card look nearly correct.

```css
/* Make the component look like a card with a shadow. */
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

/* Make the form elements lay out horizontally. */
.form-line {
  display: flex;
}

/* Stretch the form elements all the way across. */
input {
  flex: 1 0 0px;
}

/* Put a background color behind the clock image. */
.figure {
  background-color: #b3e5fc;
}

/* Size the image to the card. */
.figure img {
  width: 100%;
}

/* Put some padding in the card. */
.content {
  padding: 8px 16px;
}

/* Put the button in the correct place. */
.actions {
  padding: 8px 16px;
  display: flex;
  justify-content: flex-end;
}
```

That styles everything except the `<input>` tags. You
want those styles to be global, to apply to every
`<input>` of type text and password in the
application, so you decide to do it once and add the
following css at the end of the `styles.css` file.

```css
input::placeholder {
  font-weight: 300;
  color: rgba(0, 0, 0, .24);
}

input[type=text],
input[type=password] {
  border: 2px solid transparent;
  border-bottom-color: #eeeeee;
  padding: 8px 0 4px 0;
  outline: 0;
  background-color: transparent;
  box-sizing: border-box;
  height: 56px;
  margin-bottom: 8px;
}

input[type=text]:focus,
input[type=password]:focus {
  border-bottom-color: #ff6d00;
}
```

![timely - login card styled](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/login-card-with-style.png)

## What Did You Do?

You just reinforced everything that you learned in the
last lesson. This is an easy way that you can begin
each component by following those steps. You reflect
on what you just did.

1. Generated the `login-card` component;
1. Included the component in the application
   component's HTML file;
1. Added the component's HTML to its `.html` file;
1. Added the component-specific CSS to its `.css`
   file; and,
1. Added the site-wide CSS to the `style.css` file.

You feel good about the work you've done, so far. You
know that you have two more steps to get this to a
completely working state where it will actually
authenticate someone trying to log in. You decide to
work on the data binding so that the values of the
`<input>`s are tied to the TypeScript object.
