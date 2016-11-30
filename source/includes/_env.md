
# Environment API

> All API calls are easily chainable

```javascript

env.init()
  .then(env.shell("labVm","echo hello world"));

```

The Environment API consists of functions that allow users to create, modify and run commands on docker containers, reliant on the docker remote API. All functions are promise.js based and can be chained very easily with <code>.then</code> commands.

```javascript

env.init()
  .then(env.shell("labVm","echo hello world"),
  /*failure case*/
  function(err){ console.log(err);
  })
  .catch(/*general failure case*/);
```

Similarly, errors can be thrown and caught either at individual steps or after full execution -or both-, using the promise.js standard of either providing a failure case in the <code>Promise.then</code> case, or using a <code>.catch</code> statement

## Env.init()

Initializes the environment for a unique student, creating a lab Virtual Machine for each student and allowing them to connect to it remotely, via their own terminal or our browser interface

## Env.start()

Is a placeholder for chaining.

## Env.shell()



```javascript
env.start()
  .then(env.shell("labVm","echo hello world"))
  .then(function(sOut, sErr){ console.log(sOut,sErr);});

```

> Don't forget, you can just clone git repositories.

```javascript
env.start()
  .then(env.shell("labVm","cd /"))
  .then(env.shell("labVm","git clone https://github.com/learnlinux/tuxlab-app"))
  .then(env.shell("labVm","./deploy.sh"));
```

Env.shell allows running classic shell commands in any Virtual Machine created using the Env API. The call Env.shell(vmName, command) results in a promise, resolved with standard out, standard error or rejects with null and an error.
Every command in the student environment, aside from creating, updating, removing or networking virtual machines are meant to be implemented through env.shell().
<aside class = "success">Remember, you can always clone your files from github and run them</aside>

 Keep in mind that env.shell can easily be used to clone a git repository of your choice and run any executable you would like to put there since env.shell is capable of running any unix command. Including git clone and many others!


## Env.createVm()

```javascript
env.start()
  .then(env.createVm({createOptions: {name: "Jonathan"}});
```

Env.createVm creates a virtual machine in addition to the labVm. Although not necessary for many labs, creating multiple virtual machines may allow the creation of more versatile labs. Using Env.network -to be implemented in beta- you can network together any number of Virtual Machines in whatever way you wish to create many different labs such as SSH-connection, server-hacking ...

## Env.updateVm()

```javascript
var opts = {
  attachStdErr: true,
  name: "Jeff",
  CMD: ["/bin/sh"]
}

env.start()
 .then(env.updateVm("vm1",opts));
```

Env.update vm, updates a virtual machine with the given options. Although it can work with the labVm and all other Vms, we highly suggest not using it to update labVm as this may affectthe student connection

<aside class = "warning">Using updateVm on the labVm may result in unexpected behavior!</aside>

## Env.removeVm()

```javascript
env.start()
  .then(env.removeVm("Jeff"));
```
Removes any virtual machine except the labVm. It is suggested that you use the removeVm call to remove virtual machines after they'll no longer be used to save resources. All virtual machines are terminated after a pre-determined countdown or when the student completes the lab.
