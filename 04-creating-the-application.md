# Getting Started: Create an Angular Application

You hate starting a new project, setting up files and
directories and configuration and build scripts, it's
just so tedious! You scowl at the screen for a moment.
Then, your eyes widen and the scowl disappears,
replaced by a sly smile. You remember that you
installed the Angular command line interface (CLI)! It
does all that boring work for you!

You quickly open a command prompt and type

```bash
ng new timely-ui
```

and the CLI installs a whole slew of stuff for you in
a new `timely-ui` directory that it created. When it
finishes, you use Visual Studio Code to open the
directory by typing

```bash
code timely-ui
```

and the editor opens showing all of the files that it
created on your behalf.

You go back to the command line and type

```bash
npm start
```

to start the new application, open your browser and
type in `http://localhost:4200`, and see the following
placeholder content.

![timely - angular placeholder](https://tiy-corp-train.github.io/newline-media/learning-angular-with-timely/placeholder-angular-content.png)

*Well, that's boring*, you think. You go back to your
text editor and look at all the files that it created
for you.

## Anatomy of an CLI-Generated Angular Application

In the top level of the applications directory, you
see these files. You can figure out what they do from
their names and what you see in them.

* `README.md` - Just the read-me file for Angular.
  Boring.
* `karma.conf.js` - You remember from another project
  that `karma` is a test runner for JavaScript, so it
  makes sense that Angular, with its emphasis on
  testing, has some test runner built into the
  skeleton project. You have the inkling that you will
  write some tests before you're done.
* `package-lock.json` - NPM junk.
* `package.json` - NPM package junk.
* `protractor.conf.js` - You
  [duckduckgo](http://duckduckgo.com/) search this and
  find out that it's an end-to-end testing framework
  for Angular. You might end up writing some of that,
  too.
* `tsconfig.json` - You know
* `tslint.json` -
