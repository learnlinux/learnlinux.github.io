# Labfiles

---
### What is a Labfile?
A labfile is a Javascript application, written using the Lab API which
instructs the TuxLab server on how to setup, and step users through a lab.

You can find tons of examples on the [course repository](https://github.com/learnlinux/tuxlab-courses).

---
### Anatomy of a Labfile
A labfile is comprised of four main blocks, each which handle part of the Lab lifecycle:

!> It is extremely important to note that there are no global variables in our scope.  The
Labfile must be **stateless**, as a single instance can be used by multiple users,
and users may use different instances at different parts of the lab.

#### Constructor
This block defines various properties of the lab, including the name,
description, [LabVM (more details on the VM page)](./vm).

```javascript
Lab = new TuxLab({
  name : "GitHub Lab",
  description: "Teaches users how to clone a repository.",
  vm: 'alpine'
});
```

!> The output of the constructor **MUST** be assigned to the global `Lab`.  This is
how the server executor knows how to find it.

#### Init (optional)
Init defines a function which is run before the lab is begun.  This should
setup folders, install programs, and initialize user data.

```javascript
Lab.init(function(env){
  env.next();
});
```

#### Tasks
Tasks define what the user is to be doing in the lab.  Each task is comprised of
two functions; a setup function executed before the lab begins, and a verify function
which determines the appropriate action based on the users' performance.

In addition, the comments preceding task blocks (when written using the notation provided)
are interpreted as Markdown instructions for the user. This makes it easy to write a lab
as a single file:

```javascript
  /* @Task Name
     Description of task.  
  */
  Lab.nextTask({
    setup: function(env){
      env.next();
    },
    verify: function(env){
      env.next();
    }
  });
```

#### Destroy (optional)
This block handles destruction of the Lab:

```javascript
Lab.destroy(function(env){
  env.next();
});
```
---

### Environment Object
In the examples above, you probably noticed a mysterious `env` object.  This is
the environment object, which contains methods for controlling the lab containers
and proceeding with the lab.  Depending on the context it is called, it has different
options.  Take the `verify` context for example:

##### Container Functions
* env.shell
executes a shell command and returns a promise of {stdout : string, stderr: string}

##### Data Functions

##### Continuation Functions
* env.next
proceeds to the next task (success)
* env.retry
allows the user to retry the task
* env.failed
immediately ends the lab and alerts the user
* env.error
immediately ends the lab and reports the error to log and the user


You can read about options for other contexts in the API below.

---
### Labfile Tips and Tricks
While a Labfile is just ordinary Javascript, we have a few tips and tricks to
help get you started:

1. Use [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
Much of the Labfile API is build around Promises, as they are stylistically
preferable to callbacks.  Be careful to use them appropriately.

2. Try using [Typescript](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
Typescript is a language which extends the features of Javascript to include
Typings, as well as polyfills for the older Node.JS version used by Meteor.  We use
Typescript to create the courses in the [course repository](https://github.com/learnlinux/tuxlab-courses).

3. Debug using the Console
Any errors at compilation (when you drag the file into the course) and at execution
(as you run the lab) will be forwarded to your browser's console:

* [Chrome](https://developers.google.com/web/tools/chrome-devtools/console/)
* [Firefox](https://developer.mozilla.org/en-US/docs/Tools/Browser_Console)

---
### [Labfile API](https://github.com/learnlinux/tuxlab-app/blob/beta/imports/api/tuxlab-api.d.ts)
