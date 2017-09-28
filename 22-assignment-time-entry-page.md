# The Time Entry Page

![timely - the time entry page](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/timely-work-1.png)

You see the time entry page and, all of a sudden,
everything seems to snap into place about how to build
it.

Like the reports page, you see you'll need the clients
data service and, of course, the time entries data
service.

You see that if the person has no clients, you'll want
to show a nice message telling them to go make some
clients.

You see that, when the component initializes, you want
the component to use the client service and the time
entries data service to get the list of clients and
list of entries, respectively.

You see that, if a time entry has no end time, that you
will want to show a button and that, when clicked, will
call a method on the controller that will call the
`complete` method fo the time entries service with the
`id` value of the time entry.

You see that, after rendering all the entries for the
day, you will want to create a dropdown and a start
button that should work a lot like the report screen.
When the person clicks the start button, then it should
call a method on the controller that will call the
`create` method of the time entry service.

A little `*ngIf`, a sprinkle of `*ngFor`, a dash of
`(click)` and you think you can get this worked out in no
time!

## Hints, If You Need Them

### You can't get the data services into the time entry component?

You need to do some dependency injection.

```typescript
import { Component, OnInit } from '@angular/core';
import { TimeEntriesDataService } from 'app/time-entries-data/time-entries-data.service';
import { ClientDataService } from 'app/client-data/client-data.service';

@Component({
  selector: 'app-time-entries',
  templateUrl: './time-entries.component.html',
  styleUrls: ['./time-entries.component.css']
})
export class TimeEntriesComponent implements OnInit {

  constructor(
    private entriesData: TimeEntriesDataService,
    private clientData: ClientDataService
  ) { }

  ngOnInit() {
  }

}
```


### Forgot how to show something based on a condition?

Use `*ngIf`: https://angular.io/guide/structural-directives#ngif-case-study

### Forgot how to handle a button click?

Use `(click)`: https://angular.io/guide/template-syntax#event-binding---event-

### Forgot how to loop over a list?

Use `*ngFor`: https://angular.io/guide/structural-directives#inside-ngfor
