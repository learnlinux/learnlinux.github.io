# Lab API
TuxLab is centered around the concept of a "Lab"- which consists of 1) a series of instructions presented to the user 2) a Linux Environment setup to complete these tasks and 3) a set of tests to verify the students' completion of a task, as well as access student data.  All of these things are defined in a single Javascript file- called a LabFile.  A LabFile follows a particular format, described below.


## Creating the Lab

```javascript

var gitLab1 = new lab();
```

Every lab file is a modification on a lab object, created and initialized on the server side. This gives the tuxlab API the ability to change and adapt to advances in the server side code and other tools being used, without needing to modify any specific lab file.

Thus, all labfiles must start with creating a new lab object. By running the command <code>var myLab = new lab();</code>

### Setup

```javascript
var setup = function(env){
  env.init()
}
```

Setup is a function that takes as an input an environment object. The setup function runs for each individual user before the lab is started. Therefore, it is essential that it starts with an e<code>env.init()</code> call which initializes the environment for the user.

Although the setup function can consist on only env.init(), it is important to provide as many of the tools, files and folders a student might use in the setup function to increase efficiency.

### Tasks


Tasks are the individual steps a student goes through. Instructors can create as many tasks per lab and grade them and give feedback to students directly from the labfile using the student API.
Each task is created by a lab.newTask method, taking as input the title, a setup and verifier functions.

#### Task setup

```javascript
var setup = function(env){
  env.start()
    .then(env.shell("labVm","git clone https://github.com/cemersoz/gitlab-setup1")
    .then(env.shell("labVm","./gitlab-setup1/deploy.sh"))
    .then(env.shell("labVm","rm -rf ./gitlab-setup1"));
}
```

Each task setup is a function, taking as paramater an environment object. This function is called for each student environment to setup the tasks for each student. Every setup function MUST start with a <code>return env.start()</code> statement, although other steps can be chained to the end. It is crucial for the API to have the setup function return an environment promise.
<aside class = "notice">The setup function is run before the task is attempted.</aside>

It is important to note that while cloning git repositories is possible in the setup, it takes individual time and data connection for each user which are crucial and expensive resources. We suggest using regular unix commands and not relying on git as much as possible.

### Task verify

```javascript
var verifier = function(env,student){
  return env.start()
    .then(env.shell("labVm","pwd /"))
    .then(function(sOut,sErr){
      if(sOut.includes("my_dir")){
        student.setGrade(1,[2,2]);
        return env.resolve();
      }
    },function(){
      student.setGrade(1,[0,2]);
      student.feedback(1,"# Task failed\n## you did not create the directory \"my_dir\"");
      return env.reject();
    });

};
```

Each task verify is a function taking as parameter an environment object and a student object. The verifier is run when a student clicks the <code>verify</code> button. A verifier function must result either in an <code>env.resolve()</code> or an <code>env.reject()</code> statement.

<aside class = "notice">All verifiers must either resolve or reject the progress</aside>

For graded tasks, it is important to set the grades of the students in the verifier, depending on their progress. It is also possible to give situation-specific feedback, in markdown format, using the student.feedback function call.


#### Task Description
```javascript
/*@Task1
# This is Task1
## You can write markdown as a comment!
### Isn't that cool!
*/
```

Task descriptions are written in the markdown format, also in the labfile. You can directly write your description, anywhere on the labfile, starting with <code>@task_name</code> in any multi-line comment. All task description are parsed out from labfiles by our regular expression matcher and added to the database! Coming with the beta release, you can add your markdown descriptions in our very own markdown formatter!


### Creating and Chaining Tasks

> Lets create a task with the setup and verifier from above.

```javascript

var task1 = lab.newTask("mkdirTask",setup,verifier);
```

Tasks are created by the lab.newTask call, with paramaters taskName, setup and verifier functions.

> Lets pretend we have other functions and create more tasks...

```javascript
var task2 = lab.newTask("cpTask",setup2,verifier2);
var task3 = lab.newTask("mvTask",setup3,verifier3);
var task4 = lab.newTask("rmTask",setup4,verifier4);

```
> Now lets chain our tasks in the tasks() method...

```javascript
var tasks = function(){
  mylab.init()
    .nextTask(task1)
    .nextTask(task2)
    .nextTask(task3)
    .nextTask(task4);
}
```
Tasks are chained using the nextTask method, in a function named tasks. Task chains should start with a lab.init() call.

### Export


```javascript
module.exports = myLab;
```

At the end of the lab file, You must export the lab object you created and modified, exporting it in the standard npm way with the <code>module.exports</code> command.

## Manipulating the VM
In order to setup, and control the VM, TuxLab provides access to the `env` object, which allows you to control the VM, and even create other VMs:

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

### Env.init()

Initializes the environment for a unique student, creating a lab Virtual Machine for each student and allowing them to connect to it remotely, via their own terminal or our browser interface

### Env.start()

Is a placeholder for chaining.

### Env.shell()



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


### Env.createVm()

```javascript
env.start()
  .then(env.createVm({createOptions: {name: "Jonathan"}});
```

Env.createVm creates a virtual machine in addition to the labVm. Although not necessary for many labs, creating multiple virtual machines may allow the creation of more versatile labs. Using Env.network -to be implemented in beta- you can network together any number of Virtual Machines in whatever way you wish to create many different labs such as SSH-connection, server-hacking ...

### Env.updateVm()

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

### Env.removeVm()

```javascript
env.start()
  .then(env.removeVm("Jeff"));
```
Removes any virtual machine except the labVm. It is suggested that you use the removeVm call to remove virtual machines after they'll no longer be used to save resources. All virtual machines are terminated after a pre-determined countdown or when the student completes the lab.


## Accessing Student Data
Inside of the functions described in the LabFile, you are given access to a Student object, which allows you to save data to the Users' account, provide the user feedback, and save grades for the user.

### Student.feedback()

```javascript
var md = `
  # This is an example feedback MD
  ## It works in full markdown
  You can do most things with it
  `;

student.feedback(1,md);
```
Shows the feedback markdown provided to the student below the original task markdown once the verifier function runs. It is recommended that this is only called in the verifier function.

### Student.setGrade()

```javascript
student.setGrade(3,[2,4])
```
Sets the grade of the student for a specific task to the given tuple. Takes two parameters, taskNo, gradeTuple. Grade is set to tuple[0] out of tuple[1]

### Student.incrementGrade()

```javascript
student.incrementGrade(3,2);
```
increments the grade of the student for a specific task by the given number of points

### Student.getTaskData()

```javascript
env.start()
  .then(student.getTaskData(2))
  .then(function(data){console.log(data)});
```
Like env functions, student.getTaskData can only be called as part of a chain starting with an env.init() or env.start() call. It is a promise based function that returns a promise that either resolves with the data or rejects. For non-corrupt databases, rejecting should not be an issue. The call takes as a parameter the index -from 1- for which the data is asked.

### Student.setTaskData()

```javascript
student.setTaskData(2, data);
```
asynchronously sets the task data to the data provided. The first argument provided in the call is the task number -starting from 1- for the task to modify.

### Student.getLabData()

```javascript
env.start()
  .then(student.getLabData())
  .then(function(data){ console.log(data) });
```

Like env functions, student.getLabData can only be called as part of a chain starting with an env.init() or env.start() call. It is a promise based function that returns a promise which either resolves with the lab data or rejects. For non-corrupt databases, rejecting should not be an issue.

### Student.setLabData()

```javascript
student.setLabData(data);
```
Asynchronously sets the lab data to the data provided.

## Examples
To make this more clear, we provide a number of examples:
