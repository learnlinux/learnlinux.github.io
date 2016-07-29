
# Lab File

Every lab file is a file written in javascript, that outlines the lab a student will go through. A lab file should always have setup and tasks functions, run to setup the lab environment and tasks.

## Creating the Lab

```javascript

var gitLab1 = new lab();
```

Every lab file is a modification on a lab object, created and initialized on the server side. This gives the tuxlab API the ability to change and adapt to advances in the server side code and other tools being used, without needing to modify any specific lab file.

Thus, all labfiles must start with creating a new lab object. By running the command <code>var myLab = new lab();</code>

## Setup

```javascript
var setup = function(env){
  env.init()
}
```

Setup is a function that takes as an input an environment object. The setup function runs for each individual user before the lab is started. Therefore, it is essential that it starts with an e<code>env.init()</code> call which initializes the environment for the user.

Although the setup function can consist on only env.init(), it is important to provide as many of the tools, files and folders a student might use in the setup function to increase efficiency.

## Tasks


Tasks are the individual steps a student goes through. Instructors can create as many tasks per lab and grade them and give feedback to students directly from the labfile using the student API. 
Each task is created by a lab.newTask method, taking as input the title, a setup and verifier functions.

### Task setup

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


### Task Description
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

## Export


```javascript
module.exports = myLab;
```

At the end of the lab file, You must export the lab object you created and modified, exporting it in the standard npm way with the <code>module.exports</code> command.


