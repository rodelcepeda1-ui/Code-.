import java.util.*;
import java.util.stream.*;

/**
 * Student Records Data Processor (Pure Java)
 *
 * Loads a set of student records, runs a set of analysis operations
 * over them using the Stream API (map/filter/reduce/sorted), and
 * prints a clearly labeled report to the console. No functions
 * mutate the original student list — each returns new data.
 */
public class StudentRecords {

    // -----------------------------------------------------------------
    // Data model
    // -----------------------------------------------------------------

    static class Student {
        final int id;
        final String name;
        final int year;
        final String course;
        final List<Double> grades;
        final boolean enrolled;

        Student(int id, String name, int year, String course,
                List<Double> grades, boolean enrolled) {
            this.id = id;
            this.name = name;
            this.year = year;
            this.course = course;
            // defensive copy so external code can't mutate a student's grades
            this.grades = Collections.unmodifiableList(new ArrayList<>(grades));
            this.enrolled = enrolled;
        }
    }

    // -----------------------------------------------------------------
    // Core analysis functions
    // -----------------------------------------------------------------

    /**
     * Returns a student's average grade. A student with no grades
     * averages to 0 rather than throwing or returning NaN.
     */
    static double getAverageGrade(Student student) {
        if (student == null) {
            throw new IllegalArgumentException("getAverageGrade: student cannot be null.");
        }
        if (student.grades == null || student.grades.isEmpty()) {
            return 0.0;
        }
        return student.grades.stream()
                .mapToDouble(Double::doubleValue)
                .reduce(0.0, Double::sum) / student.grades.size();
    }

    /**
     * Returns the top n students sorted by average grade, descending.
     * Does not mutate the original list.
     */
    static List<Student> getTopStudents(List<Student> students, int n) {
        if (students == null) {
            throw new IllegalArgumentException("getTopStudents: students list cannot be null.");
        }
        if (n < 0) {
            throw new IllegalArgumentException("getTopStudents: n cannot be negative.");
        }

        return students.stream()
                .sorted(Comparator.comparingDouble(StudentRecords::getAverageGrade).reversed())
                .limit(n)
                .collect(Collectors.toList());
    }

    /**
     * Groups students by their course field.
     */
    static Map<String, List<Student>> groupByCourse(List<Student> students) {
        if (students == null) {
            throw new IllegalArgumentException("groupByCourse: students list cannot be null.");
        }

        return students.stream()
                .collect(Collectors.groupingBy(
                        student -> student.course == null ? "Unspecified" : student.course,
                        LinkedHashMap::new,
                        Collectors.toList()
                ));
    }

    /**
     * Returns a count of enrolled vs not-enrolled students.
     */
    static Map<String, Integer> getEnrolledCount(List<Student> students) {
        if (students == null) {
            throw new IllegalArgumentException("getEnrolledCount: students list cannot be null.");
        }

        long enrolled = students.stream().filter(student -> student.enrolled).count();
        long notEnrolled = students.size() - enrolled;

        Map<String, Integer> result = new LinkedHashMap<>();
        result.put("enrolled", (int) enrolled);
        result.put("notEnrolled", (int) notEnrolled);
        return result;
    }

    /**
     * Case-insensitive search for a student by name.
     * Returns null if no student matches.
     */
    static Student findStudent(List<Student> students, String name) {
        if (students == null) {
            throw new IllegalArgumentException("findStudent: students list cannot be null.");
        }
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("findStudent: name cannot be null or empty.");
        }

        String target = name.trim();
        return students.stream()
                .filter(student -> student.name != null && student.name.equalsIgnoreCase(target))
                .findFirst()
                .orElse(null);
    }

    /**
     * Returns each course's average grade, sorted highest to lowest.
     */
    static Map<String, Double> getCourseAverages(List<Student> students) {
        if (students == null) {
            throw new IllegalArgumentException("getCourseAverages: students list cannot be null.");
        }

        Map<String, List<Student>> grouped = groupByCourse(students);

        Map<String, Double> averages = new LinkedHashMap<>();
 
