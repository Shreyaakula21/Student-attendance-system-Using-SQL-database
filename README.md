# Student-attendance-system-Using-SQL-database
-- Student Attendance System (Basic SQL Version)
-- Run in MySQL

-- 1. Create database
DROP DATABASE IF EXISTS student_attendance;
CREATE DATABASE student_attendance;
USE student_attendance;

-- 2. Students table
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    roll_number VARCHAR(20) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50),
    year INT
);

-- 3. Courses table
CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_code VARCHAR(20) NOT NULL UNIQUE,
    course_name VARCHAR(100) NOT NULL
);

-- 4. Attendance table
CREATE TABLE attendance (
    attendance_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    attendance_date DATE NOT NULL,
    status ENUM('P','A') NOT NULL, -- P = Present, A = Absent
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id) ON DELETE CASCADE
);

-- 5. Insert sample students
INSERT INTO students (roll_number, name, department, year) VALUES
('R20EC001', 'Shreya Akula', 'ECE', 4),
('R20EC002', 'Amit Kumar', 'ECE', 4),
('R20EC003', 'Priya Sharma', 'ECE', 4);

-- 6. Insert sample courses
INSERT INTO courses (course_code, course_name) VALUES
('EC401', 'Embedded Systems'),
('EC402', 'IoT Fundamentals');

-- 7. Insert sample attendance records
INSERT INTO attendance (student_id, course_id, attendance_date, status) VALUES
(1, 1, '2025-08-09', 'P'),
(2, 1, '2025-08-09', 'A'),
(3, 1, '2025-08-09', 'P'),
(1, 2, '2025-08-09', 'P'),
(2, 2, '2025-08-09', 'P');

-- 8. View all attendance records
SELECT s.roll_number, s.name, c.course_code, c.course_name, a.attendance_date, a.status
FROM attendance a
JOIN students s ON a.student_id = s.student_id
JOIN courses c ON a.course_id = c.course_id
ORDER BY a.attendance_date DESC, s.roll_number;
