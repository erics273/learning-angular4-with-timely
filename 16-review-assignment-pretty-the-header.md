# Review Pretty the Header

You did a great job at prettying the header! Here's
one way you could have done it.

In `main-screen.component.html`:

```html
<header class="app-bar">
  <h1 class="app-bar-title">Timely</h1>
  <div class="app-bar-spacer"></div>
  <button class="button" (click)="logout()">Logout</button>
</header>
<nav class="tabs">
  <ul class="tabs-list">
    <li class="tabs-item tabs-item-selected"><a href="/">Today's Work</a></li>
    <li class="tabs-item"><a href="/clients">Clients</a></li>
    <li class="tabs-item"><a href="/report">Report</a></li>
  </ul>
</nav>
```

In `styles.css`:

```css
/* You can add global styles to this file, and also import other style files */
@import url('https://fonts.googleapis.com/css?family=Roboto:300,400');

html, body {
  height: 100%;
  padding: 0;
  margin: 0;
}

body {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  background-color: #f5f5f5;
}

body, input, textarea, select, button {
  font-family: 'Roboto', sans-serif;
}

body, button {
  font-size: 14px;
}

input, textarea, select {
  font-size: 16px;
}

h1, h2, h3, h4, h5, h6 {
  font-size: 1rem;
}

a.button,
button {
  border: 0;
  background-color: transparent;
  text-transform: uppercase;
  color: #ff6d00;
  padding: 4px 8px;
  text-decoration: none;
}

a.button:hover,
button:active {
  background-color: #eeeeee;
}

a.button[disabled],
button[disabled] {
  color: #eeeeee;
}

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

/* NEW STUFF THAT STYLES THE HEADER OF THE APP */
.app-bar {
  height: 64px;
  background-color: #12005e;
  color: white;
  display: flex;
  align-items: center;
}

.app-bar button {
  color: inherit;
}

.app-bar form {
  flex: 0 0 auto;
}

.app-bar-title {
  margin: 0 0 0 72px;
  font-weight: 300;
  font-size: 2em;
}

.app-bar-spacer {
  flex: 1 0 0px;
}

.tabs {
  background-color: #4a148c;
  color: white;
}

.tabs-list {
  margin: 0;
  padding: 0;
  list-style: none;
  display: flex;
}

.tabs-item {
  margin: 0;
  padding: 0;
  border-bottom: 2px solid transparent;
  display: flex;
}

.tabs-item a {
  justify-content: center;
  align-items: center;
  display: flex;
  height: 48px;
  padding: 0 24px;
  text-decoration: none;
  color: inherit;
  text-transform: uppercase;
}

.tabs-item-selected {
  border-bottom-color: #ffc400;
}
```
