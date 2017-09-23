# Nesting Components

You really enjoyed creating those components. But, as
you look at your login page, you see two components
living on the same page, the `login-card` and the
`registration-cta` components. They really belong
together since someone using the software should only
see them together according to the current UI design. And,
eventually, you're going to want to make both of them go
away so you can show the sign-up component that you just
created. So, you decide to work toward that.

## Generate the Login Page Component

You use the Angular CLI to generate a new component named
`login-page`.

```bash
npm run ng generate component login-page
```

That generated the normal files for us.

```bash
create src/app/login-page/login-page.component.css (0 bytes)
create src/app/login-page/login-page.component.html (29 bytes)
create src/app/login-page/login-page.component.spec.ts (650 bytes)
create src/app/login-page/login-page.component.ts (284 bytes)
update src/app/app.module.ts (851 bytes)
```

## Put the Other Two Components Into the Login Page Component

You open the `login-page.component.html` file and put the
Login Card and Registration Call-To-Action components in
there.

```html
<app-login-card></app-login-card>
<app-registration-cta></app-registration-cta>
```

## Put the new Login Page Comopnent Back in the Main App HTML

You change the `app-component.html` to read this.

```html
<!-- Hello, my new component! -->
<app-login-page></app-login-page>
```

Your page refreshes and everything looks all right except
for the layout. You inspect the elements and find that the
`app-login-page` is affected by the flex layout but
doesn't have any layout for itself. *Duh*, you think to
yourself, *there's nothing in the
`login-page.component.css` file!* So, you put some stuff
in the `login-page.component.css` file to apply the same
layout that you had before.

```css
:host {
  display: flex;
  flex-direction: column;
  flex: 1 0 0px;
}
```

You save your files and Everything Just Works!

## What Did You Do?

You experimented with Angular components and created
a simple parent component to render two other
Angular components.

You smile at how easy that seemed. Other than figuring out
the CSS for the parent component, everything was really
simple! You decide it's time to tackle something
potentially harder.
