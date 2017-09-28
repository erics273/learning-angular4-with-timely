# Review the Time Entry Page

Oh, my Goodness, you found bugs in your code. That's
*crazy*! But, it happens, and you worked through it
without an issue. You look at the completed page and
really like how quickly it responds to your inputs
plus the added feature of hiding the "START" button
when the person using Timely hasn't yet selected a
client.

## `time-entries.component.html`

You look back over the work you did, starting with the
`time-entries.component.html` file. You recognize that
you have started to find it easier to read, understand,
and use the `ng` stuff in Angular. That pleases you.

```html
<h1 *ngIf="clients && !clients.length" class="standalone-message">
  You must first create some clients.
</h1>
<div *ngIf="clients && clients.length">
  <table class="data-table" *ngIf="entries">
    <thead>
      <tr>
        <th class="data-table-main-column">Client</th>
        <th class="data-table-numeric">Start</th>
        <th class="data-table-numeric">End</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let entry of entries">
        <td class="data-table-main-column">{{ entry.client.name }}</td>
        <td class="data-table-numeric">{{ entry.fromFormatted() }}</td>
        <td class="data-table-numeric" *ngIf="entry.toFormatted()">{{ entry.toFormatted() }}</td>
        <td class="data-table-numeric" *ngIf="!entry.toFormatted()">
          <button (click)="finish(entry)">Finish</button>
        </td>
      </tr>
      <tr>
        <td>
          <div class="drop-down-holder single-row-chooser-main">
            <div class="drop-down-decorator">
              <i class="material-icons">arrow_drop_down</i>
            </div>
            <select [(ngModel)]="selectedClientId" class="full-width" name="id" id="clientId" required>
              <option value=""></option>
              <option *ngFor="let client of clients" [value]="client.id">{{ client.name }}</option>
            </select>
          </div>
        </td>
        <td class="data-table-numeric">
          <button (click)="create()" [disabled]="!selectedClientId" class="button-cta">Start</button>
        </td>
        <td></td>
      </tr>
    </tbody>
  </table>
</div>
```

## `time-entries.component.ts`

This is the most complex component that you've written
to date, and you feel good about it. It seems lean and
mean.

```typescript
import { Component, OnInit } from '@angular/core';
import { TimeEntriesDataService } from 'app/time-entries-data/time-entries-data.service';
import { ClientDataService } from 'app/client-data/client-data.service';
import { TimeEntry } from 'app/models/time-entry';
import { Client } from 'app/models/client';

@Component({
  selector: 'app-time-entries',
  templateUrl: './time-entries.component.html',
  styleUrls: ['./time-entries.component.css']
})
export class TimeEntriesComponent implements OnInit {

  private clients: Client[];
  private entries: TimeEntry[];
  private selectedClientId: number;

  constructor(
    private entriesData: TimeEntriesDataService,
    private clientData: ClientDataService
  ) { }

  ngOnInit() {
    this.clientData
      .getAll()
      .subscribe(clients => this.clients = clients);
    this.entriesData
      .getAll()
      .subscribe(entries => this.entries = entries);
  }

  finish(entry: TimeEntry) {
    this.entriesData
      .complete(entry)
      .subscribe(finis => {
        entry.toTime = finis.toTime;
      });
  }

  create() {
    this.entriesData
      .create(this.selectedClientId)
      .subscribe(entries => this.entries = entries);
  }

}
```

## `time-entries-data.service.js`

And, here's where you found the bugs, in the data service.
There were some wrong return types and you wanted to
change the way `TimeEntry` objects get created so you
could show a formatted from and to times. You ruefully
shake your head at the inspirations that caused you to
write that silly code.

```typescript
import { Injectable } from '@angular/core';
import { Http, RequestOptionsArgs } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import { TimeEntry, entryFactory } from 'app/models/time-entry';

@Injectable()
export class TimeEntriesDataService {

  private options: RequestOptionsArgs = {
    withCredentials: true
  };
  private baseUrl = 'http://localhost:5000/api/entries';

  constructor(private http: Http) { }

  getAll() : Observable<TimeEntry[]> {
    return this.http
      .get(this.baseUrl, this.options)
      .map(response => response.json())
      .map(entries => entries.map(entryFactory));
  }

  complete(entry: TimeEntry) : Observable<TimeEntry> {
    const id = entry.id;
    return this.http
      .post(`${this.baseUrl}/completions`, { id }, this.options)
      .map(response => response.json())
      .map((entry: TimeEntry) => entryFactory(entry));
  }

  create(clientId): Observable<TimeEntry[]> {
    return this.http
      .post(this.baseUrl, { client: { id: clientId } }, this.options)
      .map(response => response.json())
      .map(entries => entries.map(entryFactory));
  }

}
```

## `time-entry.ts`

You added the `fromFormatted` and `toFormatted` functions
onto the `TimeEntry` class. Then, you created a factory
and a utility time formatting function. This makes things
show up nicely in the UI.

```typescript
import { Client } from './client';
import { TimeWatcher } from './time-watcher';

export function entryFactory(entry: TimeEntry) {
  const newEntry = new TimeEntry();
  newEntry.id = entry.id;
  newEntry.watcher = entry.watcher;
  newEntry.client = entry.client;
  newEntry.fromTime = entry.fromTime;
  newEntry.toTime = entry.toTime;
  return newEntry;
}

export class TimeEntry {
  id: number;
  watcher: TimeWatcher;
  client: Client;
  fromTime: number;
  toTime: number;

  fromFormatted(): string {
    if (!this.fromTime) {
      return '';
    }
    return formatDate(new Date(this.fromTime));
  }

  toFormatted(): string {
    if (!this.toTime) {
      return '';
    }
    return formatDate(new Date(this.toTime));
  }
}

function formatDate(d) {
  const hours = d.getHours();
  const minutes = ('00' + d.getMinutes()).substr(-2);
  const indicator = hours > 12 ? 'PM' : 'AM';
  const hours12 = hours % 12 ? hours % 12 : '12';
  return `${hours12}:${minutes} ${indicator}`;
}
```

## What Did You Do?

You practiced all of the skills that knowledge that
you've learned! And, you know that there's just one
more to go and you're done!
