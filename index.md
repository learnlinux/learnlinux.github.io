![alt text](./tuxlab-assets/screenshots/lab_view.png "Lab View")

TuxLab is a framework for creating interactive Linux courses developed
by students of Carnegie Mellon University with the generous support
of Red Hat. It operates under a simple architectural model:

* Instructors develop "Labfiles" using Javascript, which provide instructions
  to the student, as well as code to setup and verify each step in the lab.

* Instructors can customize the lab environment by developing their own Docker
  containers, called "LabVM Images".

* Upon starting a lab, students are provisioned a Docker container, and their
  browser (or terminal if they so choose) is connected via an SSH proxy.

* Students complete a sequence of tasks defined by the instructor,
  and are provided instantaneous feedback on each step of the lab.
