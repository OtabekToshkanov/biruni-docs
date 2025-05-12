# Table structure

In order to build the courses module, several database tables are required.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

The structure shown in the diagram is essential for the **Courses Module** to manage all aspects of course planning and tracking. Here's why each part is needed:

* **`courses`** is the core, defining each course along with its assigned teacher, subject, room, schedule, and status.
* **`teachers`**, **`subjects`**, and **`rooms`** provide necessary details and constraints to assign valid combinations for a course.
* **`teacher_subjects`** ensures teachers are only assigned to subjects they are qualified to teach.
* **`students`** and **`course_students`** enable tracking of student enrollments and final grades for each course.
* **`lessons`** break down courses into individual sessions for better scheduling and attendance tracking.
* **`lesson_students`** tracks student participation and performance per lesson, giving detailed insight beyond course-level grades.
