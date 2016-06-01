---
title: API Reference

language_tabs:
  - javascript

toc_footers:
  - <a href='https://tuxlab.org'>Check out our webpage</a>
  - <a href='https://github.com/learnlinux'>View Our Source Code</a>

includes:
  - errors

search: true
---

# TuxLab

Welcome to Tuxlab! You can use our platform and APIs to create linux lessons and tutorials.


# Tuxlab API



> you can import the Tuxlab API as you would any other node module

```javascript
var tux = require('tuxlab');
var env = tuxlab.env;

//env is defined as part of the tuxlab api,
// but is easier and cleaner to use as a separate variable
```
> you can chain function calls just like promises

```javascript
env.init()
  .then(env.createVm(opts1))
  .then(env.createVm(opts2))

```

I CHANGED THIS
The Tuxlab API is based on Promise.js, all API functions return promises as explained below. Therefore, function calls can be chained as promises by the <code>.then</code> function call. Alternatively, you can use the promise-waterfall api and use function calls in a list with lambda functions.



## env.init(opts)

initializes the environment. This function creates the lab Virtual Machine, the virtual machine students connect to from their interface. All lessons should start with the env.init() call. 
<aside class="warning">
Lessons that are not initialized might result in unwanted behavior
</aside>

The parameter opts is optional, the environment defaults to a simple alpine linux virtual machine if not otherwise specified. You can specify additional options as a json file as documented on [our options page] (https://github.com/learnlinux)

## env.createVm(opts)

> to create a vm with as little options as possible
```javascript
var tux = require('tuxlab');
var env = tuxlab.env;

var vmOpts = {dockerodeCreateOptions: {name: "jonathan"}};
env.init()
  .then(createVm(vmOpts));

/*notice how we initialize the environment before creating and additional virtual machine*/
```
> to create a vm with more options specified
```javascript
var vmOpts = {
  dockerodeCreateOptions: {name: "jonathan, Image: "ubuntu"},
  dockerodeStartOptions: {}}

env.init()
  .then(createVm(vmOpts));
```

Creates and starts a new virtual machine. This command is ideal for tasks where students are expected to interact with another environment. An example usage might be a simulated website-hacking scenario.
Additional virtual machines should be connected to the Lab VM by a network. See below for instructions on how to create and manage networks. 

The opts parameter should be supplied as a json file of type
<code>{dockerodeCreateOptions:{},dockerodeStartOptions:{}}</code>
name must be specified under <code>dockerodeCreateOptions</code> all other options are optional. VMs default to an alpine linux environment. You can find our option specification document on [our options page] (https://github.com/learnlinux)

<aside class="notice">
The name of a Virtual Machine is how you access and run commands on the virtual machine. Make sure to choose distinct, explanatory names for your VMs.
</aside>


## env.updateVm(vmName,opts)
```javascript
var vmOpts = {dockerodeCreateOptions: {name: "jonathan"}};
env.init()
  .then(createVm(vmOpts))
  .then(updateVm("jonathan",{Image: "ubuntu"}));

//vm defaults to alpine linux and then is changed to ubuntu
```

Updates a virtual machine with the given options. Useful when the instructor wants to change the behavior of a virtual machine after it was created. Example: running the same task on alpine linux, and then ubuntu.
<aside class="warning">
<code>vmName</code> should be the name of a virtual machine previously defined in the environment instance running
</aside>

## env.removeVm(vmName,opts)
```javascript
var vmOpts = {dockerodeCreateOptions: {name: "jonathan"}};
env.init()
  .then(createVm(vmOpts))
  .then(removeVm("jonathan"));

//vm is created then deleted
```

Removes a virtual machine. It is efficient to remove virtual machines with idle tasks once they are no longer needed as it saves resources -and money-.
<aside class="warning">
<code>vmName</code> should be the name of a virtual machine previously defined in the environment instance running
</aside>


## env.shell(vmName,command,opts)

> it is important to check for the output
```javascript
env.init()
  .then(env.shell(labVm,"echo hello-world"))
  .then(function(sOut){ assert(sOut === "hello-world"); },
        function(sOut,sErr,dErr){ console.log(sOut);
                                  console.log(sErr);
                                  console.log(dErr);
         })
  .then(env.createVm({dockerodeCreateOptions:{name: "jonathan"}}));

//notice how we can continue chaining after handling the output
```
> it is, however, possible to forego output handling
```javascript
env.init()
  .then(env.shell(labVm,"echo hello-world"))
  .then(env.createVm({dockerodeCreateOptions:{name: "jonathan"}}));
```
Runs a given command on the virtual machine with the given name. To fully utilize <code>env.shell</code> a basic understanding of promises is helpful. A documentation and tutorial on promises can be found on [the promise.js webpage] (https://this is the link)

After a command is executed the promise is resolved with the stdout output, <code>resolve(sOut)</code> or rejected with the stdout, stderr and docker error messages, <code>reject(sOut,sErr,dockerErr)</code>

<aside class="notice">
env.shell will run if the output is not handled, but this is necessary to verify the command ran as planned
</aside>


## createTask()
## nextTask()
