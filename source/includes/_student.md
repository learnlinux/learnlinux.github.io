# Student API

The Student API allows you to set task grades and get/set data for each students' labs and tasks and give feedback to students in real time, after they submit their progress

##Student.feedback()

```javascript
var md = `
  # This is an example feedback MD
  ## It works in full markdown
  You can do most things with it
  `;

student.feedback(1,md);
```
Shows the feedback markdown provided to the student below the original task markdown once the verifier function runs. It is recommended that this is only called in the verifier function.

## Student.setGrade()

```javascript
student.setGrade(3,[2,4])
```
Sets the grade of the student for a specific task to the given tuple. Takes two parameters, taskNo, gradeTuple. Grade is set to tuple[0] out of tuple[1]

## Student.incrementGrade()

```javascript
student.incrementGrade(3,2);
```
increments the grade of the student for a specific task by the given number of points

## Student.getTaskData()

```javascript
env.start()
  .then(student.getTaskData(2))
  .then(function(data){console.log(data)});
```
Like env functions, student.getTaskData can only be called as part of a chain starting with an env.init() or env.start() call. It is a promise based function that returns a promise that either resolves with the data or rejects. For non-corrupt databases, rejecting should not be an issue. The call takes as a parameter the index -from 1- for which the data is asked.

## Student.setTaskData()

```javascript
student.setTaskData(2, data);
```
asynchronously sets the task data to the data provided. The first argument provided in the call is the task number -starting from 1- for the task to modify. 

## Student.getLabData()

```javascript
env.start()
  .then(student.getLabData())
  .then(function(data){ console.log(data) });
```

Like env functions, student.getLabData can only be called as part of a chain starting with an env.init() or env.start() call. It is a promise based function that returns a promise which either resolves with the lab data or rejects. For non-corrupt databases, rejecting should not be an issue.

## Student.setLabData()

```javascript
student.setLabData(data);
```

Asynchronously sets the lab data to the data provided.


