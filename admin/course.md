# Courses and Grading

### Creating a Course
!> **NOTE:** You must be a Global Admin to complete this step.

1. Click the "Create Course" button in the "Admin -> Courses" view.
![alt text](../tuxlab-assets/screenshots/admin_view_courses.png "Admin View => Courses")

2. (Optional) Add another user as a Course Admin by adding them to the course,
and choosing the "Course Admin" or "Instructor" roles from the dropdown.
![alt text](../tuxlab-assets/screenshots/admin_view_users.png "Admin View => Users")

---

### Adding a Lab
!> **NOTE:** You must be a Global Admin, or a Course Admin for this course to complete this step.

1. Obtain a Labfile; either by [downloading from the repository](https://github.com/learnlinux/tuxlab-courses) or [creating one yourself](../develop/lab).

2. Drag the Labfile from your desktop into the Labfile drop target:
![alt text](../tuxlab-assets/screenshots/course_view.png "Course View")

---

### Monitoring and Assisting Students in a Lab
!> **NOTE:** You must be a Global Admin, Course Admin, or Instructor for this course to complete this step.

1. Click the "User View" button in the course view.
![alt text](../tuxlab-assets/screenshots/course_view.png "Course View")

2. Click the User you wish to assist to view a list of active sessions and connection details.
![alt text](../tuxlab-assets/screenshots/course_view_users.png "Course View => Users")

---

### Exporting Grades from Lab
!> **NOTE:** You must be a Global Admin, Course Admin, or Instructor for this course to complete this step.

1. In order to export grades from a lab, click the "Export" option
![alt text](../tuxlab-assets/screenshots/course_view.png "Course View")

2. This will generate a `.zip` file containing the course records for all of your
users in `.json` format named by user name.

3. (Optional) parse the course records.  We recommend using [Python JSON tools](https://docs.python.org/2/library/json.html),
or writing a simple [Node.JS application](https://nodejs.org/en/).
