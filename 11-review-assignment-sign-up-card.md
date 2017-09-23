# Review Converting the Sign-Up Card

You did a great job on converting that sign-up card.
Here's one way to finish that assignment.

## In `app.component.html`

I replaced the content of the main app page with just my
new component so I could see it as I was developing it.
Really soon, we won't have to do that anymore.

```html
<!-- Hello, my new component! -->
<app-sign-up-card></app-sign-up-card>
```

## In `sign-up-card/sign-up-card.component.ts`

This looks strikingly similar to the
`login-card.component.ts` file. They both have two input
fields and a button and an image. Only the method name
that handles the button click and the URL to which I POST
the form has changed.

```typescript
import { Component, OnInit } from '@angular/core';
import { Http } from '@angular/http';

import 'rxjs/add/operator/catch';

@Component({
  selector: 'app-sign-up-card',
  templateUrl: './sign-up-card.component.html',
  styleUrls: ['./sign-up-card.component.css']
})
export class SignUpCardComponent implements OnInit {

  private username = '';
  private password = '';
  private error: string;

  constructor(private http: Http) { }

  ngOnInit() {
  }

  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

  submitSignup() {
    const payload = {
      username: this.username,
      password: this.password
    };
    const options = {
      withCredentials: true
    };
    const cookieUrl = 'http://localhost:5000/api/clients';
    const signUpUrl = 'http://localhost:5000/api/users';
    this.http
      .get(cookieUrl, options)
      .catch(() => this.http.post(signUpUrl, payload, options))
      .subscribe(
        () => {
          this.error = '';
          console.log('logged in');
        },
        e => {
          if (e.status === 401) {
            this.error = 'Could not sign up with those credentials';
          }
        },
      );
  }

}
```

## In `sign-up-card/sign-up-card.component.html`

This also looks strikingly similar to the
`login-card.component.html` file. They both have two input
fields and a button and an image. Only the method name
that handles the button click and the text of the button
have changed.


```html
<div class="figure"><img src="/assets/clock.png"></div>
<div class="content">
  <div class="error">{{ error }}</div>
  <div class="form-line">
    <input type="text" placeholder="username" [(ngModel)]="username">
  </div>
  <div class="form-line">
    <input type="password" placeholder="password" [(ngModel)]="password">
  </div>
</div>
<div class="actions">
  <button class="cta"
          [disabled]="disableButton"
          (click)="submitSignup()">Register!</button>
</div>
```

## In `sign-up-card/sign-up-card.component.css`

Identical to `login-card.component.css` except for the
addition at the end of the style for the "Sign up!" card
heading.

```css
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

.form-line {
  display: flex;
}

input {
  flex: 1 0 0px;
}

.figure {
  background-color: #b3e5fc;
}

.figure img {
  width: 100%;
}

.content {
  padding: 8px 16px;
}

.actions {
  padding: 8px 16px;
  display: flex;
  justify-content: flex-end;
}

.error {
  padding-top: 8px;
}

.error:empty {
  padding-top: 0;
}

.header {
  font-weight: 300;
  font-size: 2em;
}
```
