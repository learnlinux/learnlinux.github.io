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
  dockerodeCreateOptions: {name: "jonathan", Image: "ubuntu"},
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


## tuxlab.createTask(setupFn, verifyFn, opts)

> an example might be an mkdir task, asking students to create the folder "derek" in the root directory

```javascript
var setup = function(env){
  env.start()  //env.start is required if env.init is not called
    .then(env.shell("labVm","cd ~./")); //go to root directory
}
var verify = function(env){
  env.start()
    .then(env.shell("labVm","ls ~./"))
    .then(function(sOut){ if(sOut.indexOf("derek)>-1) return "success";
			  else { throw "error: no folder named derek" };},
	  function(sOut,sErr,SDock){ throw sOut + sErr + sDock; });  
}
var mkdirTask = tuxlab.createTask(setup,verify);
```

Tasks are what students follow during a lab session. Each task consists of two lambda functions, setup and verify -both implemented using the env api- the setup function runs as soon as a student starts the task to prepare the environment. The verify function runs after the students clicks the "verify" button on the interface. 

##tuxlab.init()

Tuxlab.init is the initializer function of the tuxlab environment. This function MUST be called before task appends since it tells our api which task to start with.
## task.nextTask(nTask)

> lets say we have three tasks initialized as above, task1,task2,task3

```javascript
tuxlab.init().
  .nextTask(task1)
  .nextTask(task2)
  .nextTask(task3);
```
nextTask is the function that tells our api to append tasks together. nextTask calls can be chained together much like .then calls. The example to the right shows how to specify that task1 will be run first, then task2 and finally task3.

#The LabFile

> the import

```javascript
var myLab = require('tuxlab');
```
> the setup function

```javascript
myLab.setup = function(env){ //notice how we declare an env parameter
  env.init()  //you MUST call env.init in the setup
    .then(env.createVm({cOpts:{name: "jonathan"}}));
}
```

> the tasks function

```javascript
myLab.tasks = function(env){ //notice the env parameter
  //declare tasks here
  this.init().nextTask(task1).nextTask(task2)
}
```

> the export

```javascript
module.exports = myLab;
```

The LabFile is a blueprint of a lab that instructors create. It has four basic parts and utilizes the env and tuxlab APIs documented above. The LabFile imports and extends the tuxlab api.
All LabFiles MUST start with importing the tuxlab api in a variable of your choice. This variable is then extended with two functions: setup and tasks. Both setup and tasks functions take an env object as an input, allowing you to write your code without worrying about the infrastructure connection.
<aside class="notice">
If you do not specify an env parameter in both functions, your env modify functions will cause a compile time error
</aside>

The Setup function runs as soon as a student logs in to an account. It is important to put as much as the setup as possible in the setup function to reduce loading times before and between tasks.
The tasks function runs just after setup and initializes the tasks the student will go through.
The export allows us to access the LabFile for each student and create their lab environment.
<aside class="warning">
A LabFile without an import, setup-tasks functions and an export will result in a syntax error
</aside>
