# Users and Permissions

### Users
Users can be created either by registering directly within the application,
or through a 3rd-party authentication method such as Google or Shibboleth.

---

### Course Permissions
There are four permissions options for each course:

#### Enroll Permissions
Determines who can enroll in the course:

* "Authorized User" - Only logged in users can join the course.
* "No One" - No one can join the course.

!> NOTE: If you wish to manually add students to your course, choose the
"No One" option, and add users in the Course Admin View.

#### Content Permissions
Determine who can view the content (description and labs) in a course.
Note that only logged in users can start a lab.

* "Anyone" - Anyone can view the course content.
* "Authorized User" - Only logged in users can view the course content.
* "No One" - Course is locked and visible only to instructors and administrators.


#### Meta Permissions
Determines whether a course is included in the explore and search features.

!> Can only be changed by a Global Administrator.

* true
* false

#### Featured
Whether the course is included in the "featured" section at the top of the
dashboard.

* true
* false

!> Can only be changed by a Global Administrator.

---

### User Roles
There are five distinct user roles:

* Guest
Users who aren't authenticated.

* Student
Users who have been added as a student to a course, or have begun a lab in
a course with "Anyone" enroll permissions.

* Instructor
Instructors have the ability to view course records and user sessions, but
cannot add or change labs within the course.

* Course Admin
Course administrators have the ability to edit a course; adding, changing or
removing labs.  However, they cannot add or delete courses.

* Global Admin
Global administrators have the ability to create, edit and delete courses, as well as promote
users to course administrators and instructors.
