import java.sql.*;
import java.util.Scanner;

public class StudentManagementSystem {

    // JDBC variables
    static final String JDBC_URL = "jdbc:mysql://localhost:3306/student_db";
    static final String JDBC_USER = "root";
    static final String JDBC_PASSWORD = "password";

    public static void main(String[] args) {
        try {
            // Load MySQL JDBC driver
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection connection = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD);

            Scanner scanner = new Scanner(System.in);
            int choice;

            do {
                System.out.println("Student Management System:");
                System.out.println("1. Add Student");
                System.out.println("2. View All Students");
                System.out.println("3. Update Student");
                System.out.println("4. Delete Student");
                System.out.println("5. Exit");
                System.out.print("Enter your choice: ");
                choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        addStudent(connection, scanner);
                        break;
                    case 2:
                        viewAllStudents(connection);
                        break;
                    case 3:
                        updateStudent(connection, scanner);
                        break;
                    case 4:
                        deleteStudent(connection, scanner);
                        break;
                    case 5:
                        System.out.println("Exiting...");
                        break;
                    default:
                        System.out.println("Invalid choice, please try again.");
                }
            } while (choice != 5);

            connection.close();
            scanner.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Add a new student
    public static void addStudent(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Student Name: ");
        String name = scanner.next();
        System.out.print("Enter Student Age: ");
        int age = scanner.nextInt();
        System.out.print("Enter Course: ");
        String course = scanner.next();

        String query = "INSERT INTO students (name, age, course) VALUES (?, ?, ?)";
        PreparedStatement statement = connection.prepareStatement(query);
        statement.setString(1, name);
        statement.setInt(2, age);
        statement.setString(3, course);

        int rows = statement.executeUpdate();
        if (rows > 0) {
            System.out.println("Student added successfully.");
        }
    }

    // View all students
    public static void viewAllStudents(Connection connection) throws SQLException {
        String query = "SELECT * FROM students";
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery(query);

        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("name");
            int age = resultSet.getInt("age");
            String course = resultSet.getString("course");
            System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age + ", Course: " + course);
        }
    }

    // Update student details
    public static void updateStudent(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Student ID to update: ");
        int id = scanner.nextInt();
        System.out.print("Enter new Student Name: ");
        String name = scanner.next();
        System.out.print("Enter new Age: ");
        int age = scanner.nextInt();
        System.out.print("Enter new Course: ");
        String course = scanner.next();

        String query = "UPDATE students SET name = ?, age = ?, course = ? WHERE id = ?";
        PreparedStatement statement = connection.prepareStatement(query);
        statement.setString(1, name);
        statement.setInt(2, age);
        statement.setString(3, course);
        statement.setInt(4, id);

        int rows = statement.executeUpdate();
        if (rows > 0) {
            System.out.println("Student updated successfully.");
        }
    }

    // Delete a student
    public static void deleteStudent(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Student ID to delete: ");
        int id = scanner.nextInt();

        String query = "DELETE FROM students WHERE id = ?";
        PreparedStatement statement = connection.prepareStatement(query);
        statement.setInt(1, id);

        int rows = statement.executeUpdate();
        if (rows > 0) {
            System.out.println("Student deleted successfully.");
        }
    }
}
