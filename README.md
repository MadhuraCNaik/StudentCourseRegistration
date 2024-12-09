import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Course {
    String code;
    String title;
    String description;
    int capacity;
    String schedule;
    List<Student> enrolledStudents;

    public Course(String code, String title, String description, int capacity, String schedule) {
        this.code = code;
        this.title = title;
        this.description = description;
        this.capacity = capacity;
        this.schedule = schedule;
        this.enrolledStudents = new ArrayList<>();
    }

    public boolean registerStudent(Student student) {
        if (enrolledStudents.size() < capacity) {
            enrolledStudents.add(student);
            return true;
        }
        return false;
    }

    public boolean removeStudent(Student student) {
        return enrolledStudents.remove(student);
    }

    public int getAvailableSlots() {
        return capacity - enrolledStudents.size();
    }

    @Override
    public String toString() {
        return "Course Code: " + code + "\nTitle: " + title + "\nDescription: " + description +
               "\nCapacity: " + capacity + "\nSchedule: " + schedule + "\nAvailable Slots: " + getAvailableSlots();
    }
}

class Student {
    String id;
    String name;
    List<Course> registeredCourses;

    public Student(String id, String name) {
        this.id = id;
        this.name = name;
        this.registeredCourses = new ArrayList<>();
    }

    public void registerCourse(Course course) {
        if (course.registerStudent(this)) {
            registeredCourses.add(course);
        } else {
            System.out.println("Course is full.");
        }
    }

    public void dropCourse(Course course) {
        if (course.removeStudent(this)) {
            registeredCourses.remove(course);
        }
    }

    @Override
    public String toString() {
        return "Student ID: " + id + "\nName: " + name;
    }
}

class CourseRegistrationSystem {
    Map<String, Course> courses;
    Map<String, Student> students;

    public CourseRegistrationSystem() {
        courses = new HashMap<>();
        students = new HashMap<>();
    }

    public void addCourse(Course course) {
        courses.put(course.code, course);
    }

    public void addStudent(Student student) {
        students.put(student.id, student);
    }

    public Course getCourse(String code) {
        return courses.get(code);
    }

    public Student getStudent(String id) {
        return students.get(id);
    }

    public void displayAvailableCourses() {
        for (Course course : courses.values()) {
            System.out.println(course);
        }
    }

    public void registerStudentForCourse(String studentId, String courseId) {
        Student student = getStudent(studentId);
        Course course = getCourse(courseId);
        if (student != null && course != null) {
            student.registerCourse(course);
        } else {
            System.out.println("Invalid student ID or course code.");
        }
    }

    public void removeStudentFromCourse(String studentId, String courseId) {
        Student student = getStudent(studentId);
        Course course = getCourse(courseId);
        if (student != null && course != null) {
            student.dropCourse(course);
        } else {
            System.out.println("Invalid student ID or course code.");
        }
    }

    public static void main(String[] args) {
        CourseRegistrationSystem crs = new CourseRegistrationSystem();

        // Adding courses
        crs.addCourse(new Course("CS101", "Introduction to Computer Science", "Basics of CS", 30, "Mon-Wed-Fri 10-11 AM"));
        crs.addCourse(new Course("MA101", "Calculus I", "Introduction to Calculus", 25, "Tue-Thu 9-10:30 AM"));

        // Adding students
        crs.addStudent(new Student("S001", "Alice"));
        crs.addStudent(new Student("S002", "Bob"));

        // Registering students for courses
        crs.registerStudentForCourse("S001", "CS101");
        crs.registerStudentForCourse("S002", "MA101");

        // Display available courses
        crs.displayAvailableCourses();
    }
}
