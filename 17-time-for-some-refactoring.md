# Time for Some Refactoring

### (*Stay tuned after the lesson for an unsupervised assignment*)

You look at your code and notice it's about time to
start refactoring your work because you're starting
to notice duplication and places for improvement.

One of the established tenets of object-oriented
programming is that an object should do one thing and do
it well. You look at your `login-card`, `sign-up-card`,
`main-screen` components and your `logged-in` guard and
see that they're doing something beyond their established
duties: they talk directly to AJAX services
using `$http`. You feel that you should centralize all of
those authentication-related calls in a single service
that each of your components can use.

[callout-info]
The "do one thing and do it well" saying is a colloquial
interpretation of the [single responsibility
principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)
which states that something like a component's controller
(class) should have one and only one reason to change.

**Controllers** should have one reason for existing: to
respond to actions performed by people and display
data.

For reasons like fetching data, wrapping DOM elements,
doing animation, and other concerns that aren't
directly about user interaction with data and actions,
Angular recommends using **services**.

**Services** provide common groupings of code that
components and other services can take advantage of.
For example, Angular provides the `Http` service which is
a group of methods used to make AJAX calls. You have a
group of methods spread across a bunch of classes that
deal with authentication. Hence, the need for a service.
[callout-info]

Of course, the Angular CLI has a way to generate services.
By default, the Angular CLI does not put services in their
own directory, and you want it that way. You specify the
`--flat=false` option to get the new authentication
service in its own directory under `app`.

```bash
npm run ng generate service authentication -- --flat=false
```

That spits out the following `authentication.service.ts`
file.

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class AuthenticationService {

  constructor() { }

}
```

You recognize that you need four methods on this service,
the three that the current components need to function and
the one for the guard. You create them from the logic that
already exists in the controllers.

```typescript
import { Injectable } from '@angular/core';
import { Http, Response, RequestOptionsArgs } from '@angular/http';
import { Observable } from 'rxjs/Observable';

import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch';
import 'rxjs/add/observable/of';

@Injectable()
export class AuthenticationService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  private sessionUrl = 'http://localhost:5000/api/session/mine';
  private cookieUrl  = 'http://localhost:5000/api/clients';
  private signUpUrl  = 'http://localhost:5000/api/users';

  constructor(private http: Http) { }

  login(username, password): Observable<boolean> {
    let credentials = { username, password };
    return this.getCookie()
      .catch(() => this.http.put(this.sessionUrl, credentials, this.options))
      .map(response => true)
      .catch(error => Observable.of(false));
  }

  register(username, password): Observable<boolean> {
    let credentials = { username, password };
    return this.getCookie()
      .catch(() => this.http.post(this.signUpUrl, credentials, this.options))
      .map(response => true)
      .catch(error => Observable.of(false));
  }

  logout(): Observable<boolean> {
    return this.http
      .delete(this.sessionUrl, this.options)
      .map(response =>  true)
      .catch(error => Observable.of(false));
  }

  isLoggedIn(falseFn): Observable<boolean> {
    return this.getCookie()
      .map(() => true)
      .catch((error, caught) => {
        falseFn();
        return Observable.of(false);
      });
  }

  private getCookie(): Observable<Response> {
    return this.http
      .get(this.cookieUrl, this.options);
  }

}
```

Just like other services, you need to register this so
Angular can find it. You open the `app.module.ts` file and
add it to the `providers` array just after your
`LoggedInGuard`.

Now, you go to the `LoginCardComponent` and change its
dependencies from needing `Http` to needing the
`AuthenticationService`. Then, you change the use in the
controller, too. These changes yield a
`login-card.component.ts` file that looks like this.

```typescript
import { Component, OnInit } from '@angular/core';
import { AuthenticationService } from '../authentication/authentication.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-login-card',
  templateUrl: './login-card.component.html',
  styleUrls: ['./login-card.component.css']
})
export class LoginCardComponent implements OnInit {

  private username = '';
  private password = '';
  private error: string;

  constructor(private auth: AuthenticationService, private router: Router) { }

  ngOnInit() {
  }

  get disableButton() {
    return this.username.length === 0 ||
           this.password.length === 0;
  }

  submitCredentials() {
    this.auth
      .login(this.username, this.password)
      .subscribe(
        loggedIn => {
          if (loggedIn) {
            this.error = '';
            this.router.navigate(['/main']);
          } else {
            this.error = 'Could not log in with those credentials';
          }
        }
      );
  }

}
```

## What Did You Do?

You worked out how to generate your own Angular service.
Then, you moved the authentication logic into it so that
the `LoginCardComponent` can now use it to perform its
login. You recognize that having all the authentication
logic in one place makes it easy to find and easy to
maintain in case any of the URLs change or the method by
which authentication occurs changes.

# Assignment: What More Should You Do?

You realize that you have two more components and a guard
that need to use the authentication service that you just
wrote. You feel that you should go fix them, now. So, you
do.
