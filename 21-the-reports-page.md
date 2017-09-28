# The Reports Page

You have everything you want to start building some of
the *real* pages in the application. You decide to
tackle the page that shows reports.

## Reviewing the Report Screen

You check your notes and remember that the report
screen has a drop down with a list of the logged-in
person's clients, a button to kick off the report
generation, and the actual report.

![timely - report
page](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/report.png)

You decide to tackle this in phases:

1. Use the client data service to get the person's
   clients.
1. Show an informative message if the person using
   Timely has no clients.
1. Show the client dropdown if the person has some
   clients and a disabled button.
1. Enable the button when the person selects a client.
1. React to the button click and get report data.
1. Show the report data.

## Registering the Data Services

You need client and report data services in this
component. You decide to register them with the entire
application. You open `app.module.ts`, import the three
services near the top of the document, and add them to the
`providers` list in the `@NgModule` decorator, just like
you did the `AuthenticationService` and `LoggedInGuard`.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
import { RouterModule, Routes } from '@angular/router';

import { AppComponent } from './app.component';
import { RegistrationCtaComponent } from './registration-cta/registration-cta.component';
import { LoginCardComponent } from './login-card/login-card.component';
import { SignUpCardComponent } from './sign-up-card/sign-up-card.component';
import { LoginPageComponent } from './login-page/login-page.component';
import { MainScreenComponent } from './main-screen/main-screen.component';
import { LoggedInGuard } from './logged-in.guard';
import { AuthenticationService } from './authentication/authentication.service';
import { TimeEntriesComponent } from './time-entries/time-entries.component';
import { ClientsListComponent } from './clients-list/clients-list.component';
import { ReportComponent } from './report/report.component';

// NEW AND IMPROVED (WELL, NOT IMPROVED, BUT NEW) DATA
// SERVICES FOR THE APPLICATION
import { TimeEntriesDataService } from 'app/time-entries-data/time-entries-data.service';
import { ReportDataService } from 'app/report-data/report-data.service';
import { ClientDataService } from 'app/client-data/client-data.service';


const appRoutes: Routes = [
  { path: 'login',  component: LoginPageComponent },
  { path: 'signup', component: SignUpCardComponent },
  {
    path: 'main',
    component: MainScreenComponent,
    canActivate: [LoggedInGuard],
    children: [
      { path: 'clients', component: ClientsListComponent },
      { path: 'entries', component: TimeEntriesComponent },
      { path: 'report',  component: ReportComponent },
      { path: '', redirectTo: 'entries', pathMatch: 'full' }
    ]
  },
  { path: '',  redirectTo: '/login', pathMatch: 'full' }
];

@NgModule({
  declarations: [
    AppComponent,
    RegistrationCtaComponent,
    LoginCardComponent,
    SignUpCardComponent,
    LoginPageComponent,
    MainScreenComponent,
    TimeEntriesComponent,
    ClientsListComponent,
    ReportComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(appRoutes)
  ],
  providers: [
    LoggedInGuard,
    AuthenticationService,

    // HELLO, NEW DATA SERVICES FOR THE APP
    ClientDataService,
    ReportDataService,
    TimeEntriesDataService
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Inject and Use the Client Data Service

You open `report.component.ts` and tell Angular that
you want the client data service provided for the
controller.

```typescript
import { Component, OnInit } from '@angular/core';
import { ClientDataService } from 'app/client-data/client-data.service';
import { Client } from 'app/models/client';

@Component({
  selector: 'app-report',
  templateUrl: './report.component.html',
  styleUrls: ['./report.component.css']
})
export class ReportComponent implements OnInit {

  private clients: Client[];

  constructor(private clientData: ClientDataService) { }

  ngOnInit() {
    this.clientData
      .getAll()
      .subscribe(clients => this.clients = clients);
  }

}
```

You think it'd be nice to see what that returns, so you
open `report.component.html` and replace its content
with

```handlebars
<pre>
{{ clients | json }}
</pre>
```

You open a new browser window, type in
[http://localhost:5000](http://localhost:5000) and log
in using the username "maria" with password "maria".
Then, you click on the "REPORT" tab and see the result
of the data passed back from the client.

```json
[
  {"id":22,"name":"Allie's Sweets","isActive":true},
  {"id":21,"name":"Bret's Refrigeration","isActive":true},
  {"id":20,"name":"Zantina Unlimited","isActive":true}
]
```

That's awesome!

You click the "SIGN OUT" button, refresh the page, and
log back in using the username "hector" and the
password "hector". You click on the "REPORT" tab and
see an empty list returned.

```json
[]
```

## Show an Informative Message About Not Having Clients

If "hector" doesn't have any clients, he should see the
helpful message that he has no clients. To see what
Michaela used in her template, you open the
`report/index.html` file under the `templates` section.
On line 30, you see where she displays the message for
someone that has no clients. You copy the HTML and
replace the content of `report.component.html` with it.

```html
<h1 class="standalone-message">
  You must first create some clients.
</h1>
```

You only want that message to appear if there are no
clients. To do conditional display like that, you
recall that Angular provides the `*ngIf` attribute
directive thing which works kind of like a JavaScript
`if` statement. You use the `*ngIf` to show the message
when clients has a value and that the list is empty.

```html
<h1 *ngIf="clients && !clients.length" class="standalone-message">
  You must first create some clients.
</h1>
```

When you look at the report page for "hector", you see

![timely - Angular showing no clients](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/ng-if-on-report-screen.png)

and when you look at it for "maria", you see nothing.
The `*ngIf` works as advertised! You thank the
Angular Overlords and move on to your next story.

## Show a Client Dropdown and a Disabled Button

You go back into Michaela's HTML and find the part that
shows the chooser from lines 36 - 54 in
`report/index.html`. You grab that stuff and put it
inside a `<div>` so you can control it with an `ng-if`.

```html
<h1 *ngIf="clients && !clients.length" class="standalone-message">
  You must first create some clients.
</h1>
<div *ngIf="clients && clients.length">
  <div class="single-row-chooser">
    <div class="drop-down-holder single-row-chooser-main">
      <div class="drop-down-decorator">
        <i class="material-icons">arrow_drop_down</i>
      </div>
      <select class="full-width" name="id" id="clientId" required>
        <option value=""></option>
        {{#clients}}
          {{#isSelected}}
            <option value="{{ id }}" selected>{{ name }}</option>
          {{/isSelected}}
          {{^isSelected}}
            <option value="{{ id }}">{{ name }}</option>
          {{/isSelected}}
        {{/clients}}
      </select>
    </div>
    <button class="button-cta">Generate report</button>
  </div>
</div>
```

Now, you need to change it to use Angular mechanisms
rather than `mustache` mechanisms to populate the
`<select>` element. You do a quick
[duckduckgo.com](http://duckduckgo.com) search and find
that you should just use an `*ngFor` to create a bunch of
`<option>` tags for the `<select>` and two-way binding for
the `<select>`.

```html
<h1 *ngIf="clients && !clients.length" class="standalone-message">
  You must first create some clients.
</h1>
<div *ngIf="clients && clients.length">
  <div class="single-row-chooser">
    <div class="drop-down-holder single-row-chooser-main">
      <div class="drop-down-decorator">
        <i class="material-icons">arrow_drop_down</i>
      </div>

      <!-- Bind the select to the "selectedClientId" field -->
      <select [(ngModel)]="selectedClientId" class="full-width" name="id" id="clientId" required>
        <option value=""></option>

        <!-- Loop over the clients and populate the select -->
        <option *ngFor="let client of clients" [value]="client.id">{{ client.name }}</option>
      </select>
    </div>
    <button class="button-cta">Generate report</button>
  </div>
</div>
```

## Enable the Button When Client is Selected

You realize that this is like the disabling of the
button on the login form. Except, instead of a string,
you will enable or disable the button based on the
length of a string value, you will base it on whether
the `selectedClientId` property has a value.

Because your JavaScript-fu is strong, you change that
`disabled` property on the button to an `[disabled]`
property binding and set it equal to
`not-falsey-value-of-selectedClientId`.

```html
<h1 *ngIf="clients && !clients.length" class="standalone-message">
  You must first create some clients.
</h1>
<div *ngIf="clients && clients.length">
  <div class="single-row-chooser">
    <div class="drop-down-holder single-row-chooser-main">
      <div class="drop-down-decorator">
        <i class="material-icons">arrow_drop_down</i>
      </div>
      <select [(ngModel)]="selectedClientId" class="full-width" name="id" id="clientId" required>
        <option value=""></option>
        <option *ngFor="let client of clients" [value]="client.id">{{ client.name }}</option>
      </select>
    </div>

    <!-- Add a disabled binding to make it off when nothing is selected -->
    <button [disabled]="!selectedClientId" class="button-cta">Generate report</button>
  </div>
</div>
```

## Styling the Drop Down

You head back to Michaela's style file and hunt down all
the styles needed to make the `single-row-chooser` and
all the other stuff look right and paste it to the end of
the global `styles.css` file in the `src` directory.

## Getting the Report When the Button Gets Clicked

You want to capture the click of the "GENERATE REPORT"
button, but you are having a brain fart. You can't
remember how to do it, but you remember doing it for
the "SIGN OUT" button. You open the
`main-screen.component.html` file and see that it's
`(click)`.

`(click)`! That's right!

You set up the `<button>` to call a method named
`generate` on the controller.

```html
      </select>
    </div>

    <!-- Call generate() on the component when clicked! -->
    <button (click)="generate()"
            [disabled]="!selectedClientId"
            class="button-cta">Generate report</button>
  </div>
</div>
```

You open the corresponding `.ts` file for the
component, `report.component.ts`, and add the
`generate()` method.

You realize you've come to the point where you need
Angular to give you the report data service. You
change the constructor to ask for it.

```typescript
import { Component, OnInit } from '@angular/core';
import { ClientDataService } from 'app/client-data/client-data.service';
import { Client } from 'app/models/client';
import { ReportDataService } from 'app/report-data/report-data.service';

@Component({
  selector: 'app-report',
  templateUrl: './report.component.html',
  styleUrls: ['./report.component.css']
})
export class ReportComponent implements OnInit {

  private clients: Client[];

  constructor(
    private clientData: ClientDataService,
    private reportData: ReportDataService
  ) {}

  ngOnInit() {
    this.clientData
      .getAll()
      .subscribe(clients => this.clients = clients);
  }

  generate() {
  }

}
```

You look back at the report data service to see how you
defined it. You see that you need to call the
`getReportData` method on it and pass the `id` property of
the client. This means you'll need to access the selected
client in the component's `.ts` file. You create a
`private` field to hold that information.

```typescript
import { Component, OnInit } from '@angular/core';
import { ClientDataService } from 'app/client-data/client-data.service';
import { Client } from 'app/models/client';
import { ReportDataService } from 'app/report-data/report-data.service';

@Component({
  selector: 'app-report',
  templateUrl: './report.component.html',
  styleUrls: ['./report.component.css']
})
export class ReportComponent implements OnInit {

  private clients: Client[];

  // LOOK! A way to get the selected client!
  private selectedClientId: number;

  constructor(
    private clientData: ClientDataService,
    private reportData: ReportDataService
  ) {}

  ngOnInit() {
    this.clientData
      .getAll()
      .subscribe(clients => this.clients = clients);
  }

  generate() {
  }

}
```

Now, you can make the call to the `ReportDataService`, use
the selected client, and get report data that you will
store in a new variable to hold it.

```typescript
import { Component, OnInit } from '@angular/core';
import { ClientDataService } from 'app/client-data/client-data.service';
import { Client } from 'app/models/client';
import { ReportDataService } from 'app/report-data/report-data.service';
import { ReportEntry } from 'app/models/report-entry';

@Component({
  selector: 'app-report',
  templateUrl: './report.component.html',
  styleUrls: ['./report.component.css']
})
export class ReportComponent implements OnInit {

  private clients: Client[];
  private selectedClientId: number;
  private entries: ReportEntry[];

  constructor(
    private clientData: ClientDataService,
    private reportData: ReportDataService
  ) {}

  ngOnInit() {
    this.clientData
      .getAll()
      .subscribe(clients => this.clients = clients);
  }

  generate() {
    this.reportData
      .getReportData(this.selectedClientId)
      .subscribe(data => this.entries = data);
  }

}
```

## Showing the Report Data

You go back to `report.component.html` and dump the
value of the data onto the page by adding `{{
entries | json }}` at the end of the HTML. You select a
client, click the "GENERATE REPORT" button, and JSON!

Under the default seed data, *maria*'s report for the
client Zantina Unlimited returns this data.

```json
[
  {"month":1483250400852,"monthName":"January","numberOfHoursWorked":22,"year":"2017"},
  {"month":1485928800852,"monthName":"February","numberOfHoursWorked":13,"year":"2017"},
  {"month":1488348000852,"monthName":"March","numberOfHoursWorked":6,"year":"2017"},
  {"month":1491022800852,"monthName":"April","numberOfHoursWorked":3,"year":"2017"},
  {"month":1493614800852,"monthName":"May","numberOfHoursWorked":2,"year":"2017"},
  {"month":1496293200852,"monthName":"June","numberOfHoursWorked":1,"year":"2017"}
]
```

You delete the `{{ entries | json }}` from the page and
open Michaela's `report/index.html` file, again. In that
file, you see that lines 58 - 73 contain the HTML for the
table. You copy and paste that into
`report.component.html` as the last child of the `<div>`
that shows stuff only when the user has clients.

```html
<h1 ng-if="report.clients.length === 0" class="standalone-message">
  You must first create some clients.
</h1>
<div ng-if="report.clients.length > 0">
  <div class="single-row-chooser">
    <div class="drop-down-holder single-row-chooser-main">
      <div class="drop-down-decorator">
        <i class="material-icons">arrow_drop_down</i>
      </div>
      <select class="full-width" name="id" id="clientId" required
              ng-model="report.selectedClient"
              ng-options="client.name for client in report.clients">
        <option value=""></option>
      </select>
    </div>
    <button ng-disabled="!report.selectedClient"
            ng-click="report.generate()"
            class="button">Generate report</button>
  </div>

  <!-- Hello, report table!!! I am soooo close! -->
  <table class="data-table">
    <thead>
      <tr>
        <th class="data-table-main-column">Month</th>
        <th class="data-table-numeric">Hours worked</th>
      </tr>
    </thead>
    <tbody>
      {{#report}}
        <tr>
          <td class="data-table-main-column">{{ monthName }} {{ year }}</td>
          <td class="data-table-numeric">{{ numberOfHoursWorked }}</td>
        </tr>
      {{/report}}
    </tbody>
  </table>
</div>
```

The `{{#report}}` and `{{/report}}` stuff is how
`mustache` does looping, so you delete that and
re-indent the HTML so it looks right.

```html
  <!-- Hello, report table!!! I am soooo close! -->
  <table class="data-table">
    <thead>
      <tr>
        <th class="data-table-main-column">Month</th>
        <th class="data-table-numeric">Hours worked</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="data-table-main-column">{{ monthName }} {{ year }}</td>
        <td class="data-table-numeric">{{ numberOfHoursWorked }}</td>
      </tr>
    </tbody>
  </table>
</div>
```

You only want the table to appear if you have data to
show, so you put an `ng-if` on the `<table>` to only
show if you have data.

```html
  <table class="data-table" *ngIf="entries">
```

In Angular, you've already seen that you are supposed
to use the `*ngFor` attribute directive when you
want things to repeat. You want it to repeat rows of
the table, specifically, the `<tr>`s inside the
`<tbody>`. You add the `*ngFor` and fix the output
in the `<td>`s to reference the looping variable.

```html
<tbody>

  <!-- Repeat until you're done, little rows of data! -->
  <!-- Each entry in the data will be put into the row
       variable. -->
  <tr *ngFor="let entry of entries">

    <!-- You reference the row variable, here, to get
         the values onto the page! -->
    <td class="data-table-main-column">{{ entry.monthName }} {{ entry.year }}</td>
    <td class="data-table-numeric">{{ entry.numberOfHoursWorked }}</td>
  </tr>
</tbody>
```

## What Did You Do?

Oh, my, you did so much!

* You used your custom data services by having
  Angular inject them into your component.
* You used the component lifecycle method `ngOnInit` to
  populate initial information from the client data
  service.
* You used `*ngIf` to show and not show HTML based on
  the value of properties of the controller.
* You used `*ngFor` and `ngModel` on a `<select>`
  to bind the selected value to a property on the
  controller.
* You used the selected value to call a data service.
* You used the `*ngFor` to show data from the data
  service.

That's an entire Angular education on one page! Wow!

You take a deep breath and decide to take a small
break. You look over at your goldfish whose eyes seem
to tell you *good job!*
