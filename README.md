# Student-Records-Java-JDBC


A Java console application that connects to a MySQL database, reads student records, and prints the average mark and grade of students whose names start with **J** or **M**.

## Features

- Connects to a MySQL database using JDBC
- Retrieves students whose names start with `J`, then `M`
- Calculates each student's average across 5 subjects
- Assigns a letter grade based on the average
- Shows only students who passed (average of 50 or more)
- Prints results in a formatted table

## Grading Scale

| Average | Grade |
|---------|-------|
| Above 85 | A |
| 75 to 85 | B |
| 60 to 74.99 | C |
| 50 to 59.99 | D |
| Below 50 | Not displayed |

## Requirements

- Java Development Kit (JDK) 8 or higher
- MySQL Server
- [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/) (JDBC driver)

## Database Setup

The program expects a database named `School` with a `Students` table containing a `name` column and five subject mark columns:

```sql
CREATE DATABASE School;
USE School;

CREATE TABLE Students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    subject1 DOUBLE,
    subject2 DOUBLE,
    subject3 DOUBLE,
    subject4 DOUBLE,
    subject5 DOUBLE
);

INSERT INTO Students (name, subject1, subject2, subject3, subject4, subject5) VALUES
('John',  90, 88, 92, 85, 89),
('Jane',  70, 65, 72, 68, 75),
('Mary',  80, 78, 82, 76, 79),
('Mark',  55, 52, 58, 50, 54),
('Joel',  40, 35, 45, 30, 42);
```

## Configuration

The connection settings are set in `StudentRecords.java`:

```java
Connection con = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/School", "root", "");
```

Change the URL, username, and password to match your MySQL setup.

## Getting Started

### Compile

```bash
javac StudentRecords.java
```

### Run

Include the MySQL Connector/J jar on the classpath (replace the jar name with your version):

```bash
# Windows
java -cp ".;mysql-connector-j-8.x.x.jar" StudentRecords

# Linux / macOS
java -cp ".:mysql-connector-j-8.x.x.jar" StudentRecords
```

## Sample Output

Using the sample data above:

```
STUDENTS STARTING WITH J
Name            Average      Grade
--------------------------------------
John            88.8         A
Jane            70.0         C

STUDENTS STARTING WITH M
Name            Average      Grade
--------------------------------------
Mary            79.0         B
Mark            53.8         D
```

Joel is not shown because his average is below 50.

## How It Works

1. Opens a connection to the `School` database.
2. Runs `SELECT * FROM Students WHERE name LIKE 'J%'`.
3. For each row, averages `subject1` to `subject5` and assigns a grade.
4. Prints the student if the average is 50 or higher.
5. Repeats the same steps for names starting with `M`.
6. Closes the result set, statement, and connection.

## Possible Improvements

- Move the repeated grading logic into a separate method
- Use `PreparedStatement` instead of `Statement`
- Use try-with-resources to close connections automatically
- Store database credentials in a config file or environment variables instead of the source code
- Let the user choose the starting letter instead of hardcoding `J` and `M`

## Project Structure

```
.
├── StudentRecords.java
└── README.md
```
