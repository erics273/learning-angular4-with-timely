<style>.m-coursecontent-table-outer { border: 0; }</style>

# Services! Services! Services!

You decide that before you get on with the creation of
the child states for entering time, managing clients,
and generating reports that you want to write the data
services for each of those states. To do this, you
need to look at Michaela's code.

Inside the `com.theironyard.timeliness.controllers`
package, you see the API controllers that she's
created for just such a need.

![timely - list of API controllers](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/api-controllers.png)

You decide to tackle the API for time entries, first.

## Designing the Data Service for Time Entries

You open the `TimeEntriesApiController.java` file and
look at the mappings and methods of the controller
class.

```java
@RestController
@RequestMapping("/api/entries")
public class TimeEntriesApiController {

  // Constructor and private fields

  @GetMapping("")
  public List<WorkSpan> getEntryForm(Authentication auth, Model model) {
    // Stuff
  }

  @PostMapping("/completions")
  public WorkSpan completeWorkSpan(Authentication auth, @RequestBody WorkSpan span) {
    // More stuff
  }

  @PostMapping("")
  public List<WorkSpan> startWorkSpan(Authentication auth, @RequestBody WorkSpan span) {
    // Java, java, java
  }

}
```

From the code, you note the following API.

| Method | Path                     | Return value           | Parameter               |
|--------|--------------------------|------------------------|-------------------------|
| GET    | /api/entries             | a list of `WorkSpan`s  |                         |
| POST   | /api/entries/completions | a completed `WorkSpan` | a `WorkSpan`            |
| POST   | /api/entries             | a list of `WorkSpan`s  | a new `WorkSpan` to add |

You also go look at this `WorkSpan` class that gets
referenced in this API. Ignoring the JPA annotations,
you see the following data.

```java
public class WorkSpan {

  private Long id;

  private TimeWatcher watcher;

  private Client client;

  private Date fromTime;

  private Date toTime;

  // Constructors
  // Getters and setters

}
```

That leads to a couple of other classes, too,
`TimeWatcher` and `Client`.

Playing around with Timely, you know that it sets the
`fromTime` to the current time when the `WorkSpan`
gets created. You also know that the `toTime` gets set
when you call the `/api/entries/completions` ReST
endpoint. JPA handles the `id` and the authentication
should handle the `watcher` fields. Armed with this
knowledge, you scratch down a design for a time
entries data service.

| Method   | Parameter                | Return value if successful             |
|----------|--------------------------|----------------------------------------|
| getAll   |                          | a list of time entries for the day     |
| complete | the id of the time entry | the updated time entry                 |
| create   | the id of the client     | the list of all the day's time entries |

## Creating the Model Classes

Here, Angular can't help with a model class because it
will just be a regular old TypeScript class. You create a
new directory under `app` and name it `models`. In there,
you create a three new files named `client.ts`,
`time-watcher.ts`, and `time-entry.ts` and put the following
code into each.

```typescript
export class Client {
  id: number;
  name: string;
  isActive: boolean;
}
```

```typescript
export class TimeWatcher {
  id: number;
  username: string;
}
```

```typescript
import { Client } from './client';
import { TimeWatcher } from './time-watcher';

export class TimeEntry {
  id: number;
  watcher: TimeWatcher;
  client: Client;
  fromTime: Date;
  toTime: Date;
}
```

## Coding the Time Entries Service

You generate a new service for time entries data with the
following command:

```bash
npm run ng generate service time-entries-data -- --flat=false
```

which gives you the following service file

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class TimeEntriesDataService {

  constructor() { }

}
```

### Implementing the `getAll` Method

You start off easy enough writing the `GET` portion of
the `getAll` method in the `TimeEntriesDataService`
class. But, you find yourself asking, "After invoking
the `get` method of the `Http` object, what is this
thing actually returning?"

```typescript
import { Injectable } from '@angular/core';
import { Http, RequestOptionsArgs } from '@angular/http';

@Injectable()
export class TimeEntriesDataService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  baseUrl = 'http://localhost:5000/api/entries'

  constructor(private http: Http) { }

  getAll() {
    return this.http
      .get(this.baseUrl, this.options)
      // now what?
  }

}
```

You head over to the [Angular `Http`
documentation](https://angular.io/api/http/Http) and see
that `get` returns an `Observable<Response>`. You shake
your head at this because your **data** service should
return data, not a `Response` object for an HTTP request.

The [documentation for
`Response`](https://angular.io/api/http/Response) shows
that it inherits from `Body` which has a method named
`json()` which returns an object of type `any`. Now, that
sounds a lot better to you than returning an HTTP response
object.

Well, you figure that since the `@RestController` in
Spring serializes its data as `application/json`, that
return value from `json()` response must be the list of
time entries that you want!

You finish implementing the `getAll` method with that
knowledge.

```typescript
import { Injectable } from '@angular/core';
import { Http, RequestOptionsArgs } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import { TimeEntry } from 'app/models/time-entry';

@Injectable()
export class TimeEntriesDataService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  baseUrl = 'http://localhost:5000/api/entries'

  constructor(private http: Http) { }

  getAll() : Observable<TimeEntry> {
    return this.http
      .get(this.baseUrl, this.options)
      .map(response => response.json());
  }

}
```

### Implementing the `complete` Method

You know that you need to send at least the `id` of
the time entry to Michaela's service so that it knows
which time entry to mark complete and set an end time
for that time entry. With that knowledge, you knock
out the `complete` method of your data service.

```typescript
import { Injectable } from '@angular/core';
import { Http, RequestOptionsArgs } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import { TimeEntry } from 'app/models/time-entry';

@Injectable()
export class TimeEntriesDataService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  baseUrl = 'http://localhost:5000/api/entries'

  constructor(private http: Http) { }

  getAll() : Observable<TimeEntry> {
    return this.http
      .get(this.baseUrl, this.options)
      .map(response => response.json());
  }

  complete(id) : Observable<TimeEntry> {
    return this.http
      .post(`${this.baseUrl}/completions`, { id }, this.options)
      .map(response => response.json());
  }

}
```

### Implementing the `create` Method

You refer to your design table and see that you need
to send the client id to Michaela's RESTful service
to create a new time entry. Feeling good about how
quickly you're knocking out the code, you drive it
home with the following implementation.

```typescript
import { Injectable } from '@angular/core';
import { Http, RequestOptionsArgs } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import { TimeEntry } from 'app/models/time-entry';

@Injectable()
export class TimeEntriesDataService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  baseUrl = 'http://localhost:5000/api/entries'

  constructor(private http: Http) { }

  getAll() : Observable<TimeEntry> {
    return this.http
      .get(this.baseUrl, this.options)
      .map(response => response.json());
  }

  complete(id) : Observable<TimeEntry> {
    return this.http
      .post(`${this.baseUrl}/completions`, { id }, this.options)
      .map(response => response.json());
  }

  create(clientId): Observable<TimeEntry> {
    return this.http
      .post(this.baseUrl, { client: { id: clientId } }, this.options)
      .map(response => response.json());
  }

}
```

## What Did You Do?

In this lesson, you worked in more than just the
front-end, you did some full-stack development! You

* read an existing Java controller;
* extracted the API from the controller;
* designed a data service; and,
* investigated the `http` service to implement your
  data service.

You remind yourself, though, there are two more
data services to go...
