# Career_247 – SQL Practice Project

A hands-on SQL project covering database/table creation, DML operations, filtering, aggregate functions, subqueries (Nth highest value pattern), and string functions — written and executed in **MySQL Workbench**.

## 📋 Overview

| | |
|---|---|
| **Database** | `Career_247` (MySQL) |
| **Table** | `Students` |
| **Columns (final)** | `Student_ID`, `First_Name`, `Last_Name`, `Age`, `Marks`, `City`, `Course` |

> **Notes on the data (flagging as-is, not corrected):**
> - The insert for `Ranjana` contains a typo: `"Shuk;a"` instead of `"Shukla"`. It's kept exactly as run.
> - A `Gender` column was added and later dropped. Only two students (`Param`, `Anjani`) were ever explicitly set to `"Male"`; no row was ever set to `"Female"`, so any query filtering on `Gender = "Female"` (e.g. `SUM(marks) ... WHERE Gender="Female"`) would return `NULL`/no rows against this dataset.
> - `Student_ID` values were reused after deletions (e.g. ID 106 renamed to 105 after 105 was deleted) to keep IDs contiguous — this is intentional manual re-sequencing, not an error, but worth knowing if you reuse this script.

---

## 1. Database & Table Setup

```sql
CREATE DATABASE Career_247;
USE Career_247;

CREATE TABLE Students (
    Student_ID INT(4),
    First_Name VARCHAR(20),
    Last_Name VARCHAR(20),
    Age INT(2),
    Marks INT(2),
    City VARCHAR(20)
);
```

## 2. Insert Initial Data

```sql
INSERT INTO Students VALUES
(101, "Anjani", "Tripathi", 32, 89, "Delhi"),
(102, "Shikha", "Yadav", 33, 90, "Lucknow"),
(103, "Shweta", "Singh", 32, 78, "Mau"),
(104, "Ranjana", "Shuk;a", 34, 80, "Mainpuri"),
(105, "Param", "Shukla", 32, 60, "Deoria"),
(106, "Shanvi", "Singh", 25, 79, "Jaunpur");
```

## 3. Basic SELECT & Filtering

```sql
-- View all records
SELECT * FROM Students;

-- Select specific columns
SELECT First_Name, Age, Marks FROM Students;

-- Students with marks >= 70
SELECT * FROM Students WHERE Marks >= 70;

-- Students older than 30
SELECT * FROM students WHERE Age > 30;

-- Students with marks > 80
SELECT Student_ID, First_Name, Last_Name FROM students WHERE marks > 80;

-- Students with marks > 60 AND age > 30
SELECT Student_ID, First_Name, Last_Name, Age FROM students
WHERE marks > 60 AND Age > 30;
```

## 4. Altering the Table Structure

```sql
ALTER TABLE students ADD course VARCHAR(20);
ALTER TABLE students ADD Gender VARCHAR(20);

SELECT * FROM Students;
```

## 5. Update Operations

```sql
SET SQL_SAFE_UPDATES = 0;

-- Set a default course for all students
UPDATE students SET course = "Data Analytics";

-- Set gender for specific students
UPDATE Students SET Gender = "Male" WHERE First_Name = "Param";
UPDATE Students SET Gender = "Male" WHERE First_Name = "Anjani";
```

## 6. Delete & ID Re-sequencing

```sql
-- Remove a student
DELETE FROM students WHERE Student_ID = 105;

-- Reassign the next student's ID to fill the gap
UPDATE students SET Student_ID = 105 WHERE Student_ID = 106;
```

## 7. Conditional Counting

```sql
-- How many students are older than 32?
SELECT COUNT(*) AS Student_Count FROM students WHERE Age > 32;
```

## 8. More Course Updates & Course-Based Count

```sql
UPDATE students SET course = "Data Science" WHERE First_Name = "Shanvi";
UPDATE students SET course = "Digital Marketing" WHERE First_Name = "Shikha";

SELECT COUNT(Student_ID) AS Student_Course_Count FROM students
WHERE course = "Data Analytics";
```

## 9. Aggregate Functions

```sql
SELECT SUM(marks) AS Total_Marks FROM Students;

-- Note: returns NULL against this dataset — no row has Gender = "Female"
SELECT SUM(marks) AS Total_Marks_Female FROM Students WHERE Gender = "Female";

SELECT AVG(Age) AS Avg_age FROM Students;

SELECT MAX(Marks) AS Max_Marks FROM Students;

SELECT MIN(Marks) AS Min_Marks FROM Students;
```

## 10. Subqueries – Highest & 2nd Highest Marks

```sql
-- Student(s) with the highest marks
SELECT * FROM students WHERE marks = (SELECT MAX(Marks) FROM Students);

-- Highest marks value
SELECT MAX(Marks) FROM students
WHERE marks < (SELECT MAX(marks) FROM students);   -- 2nd highest marks value

-- Full row for the student with the 2nd highest marks
SELECT * FROM students
WHERE marks = (
    SELECT MAX(marks) FROM Students
    WHERE marks < (SELECT MAX(marks) FROM Students)
);
```

## 11. Rounding & Data Cleanup

```sql
SELECT ROUND(AVG(Age), 1) AS Avg_age FROM Students;

-- Add a new student
INSERT INTO students VALUES
(106, "Shivam", "Dhoni", 26, 69, "Delhi", "Digital Marketing", "Male");

-- Remove a student and re-sequence IDs again
DELETE FROM Students WHERE Student_ID = 104;
UPDATE students SET Student_ID = 104 WHERE Student_ID = 105;
UPDATE students SET Student_ID = 105 WHERE Student_ID = 106;

SELECT COUNT(*) FROM Students WHERE Age >= 26;

-- Drop the Gender column
ALTER TABLE Students DROP COLUMN Gender;

SELECT COUNT(Student_ID) FROM Students WHERE Course = "Data Analytics";

UPDATE students SET course = "Digital Marketing" WHERE Student_ID = 105;
```

## 12. Aggregate & Rounding Functions (Combined)

```sql
SELECT MAX(marks) AS Highest_Marks, MIN(marks) AS Lowest_Marks FROM students;

SELECT ROUND(AVG(Age), 2) AS Avg_Age FROM Students;

SELECT FLOOR(AVG(Age)) FROM Students;

SELECT CEILING(AVG(age)) FROM Students;
```

## 13. String Functions

```sql
-- Concatenate first and last name
SELECT CONCAT(First_Name, " ", Last_Name) AS Full_Name, Age FROM Students;

-- Substring of first name
SELECT First_Name, SUBSTRING(First_Name, 3, 4) FROM Students;

-- Character length of first name
SELECT First_Name, LENGTH(First_Name) AS Char_Count FROM Students;

-- Character count of full name (minus the space)
SELECT CONCAT(First_name, " ", Last_Name) AS Full_Name,
       LENGTH(CONCAT(First_name, " ", Last_Name)) - 1 AS Actual_Char_Count
FROM Students;

-- Proper-case formatting of first name
SELECT First_Name,
       CONCAT(UPPER(LEFT(First_Name, 1)), LOWER(SUBSTRING(First_Name, 2, LENGTH(First_Name)))) AS Proper_Case
FROM Students;
```

---

## Concepts Covered

- **DDL:** `CREATE DATABASE`, `CREATE TABLE`, `ALTER TABLE` (add/drop column)
- **DML:** `INSERT`, `UPDATE`, `DELETE`
- **Filtering:** `WHERE`, compound conditions (`AND`)
- **Aggregate functions:** `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`
- **Subqueries:** Nth-highest-value pattern (2nd highest marks)
- **Numeric functions:** `ROUND`, `FLOOR`, `CEILING`
- **String functions:** `CONCAT`, `SUBSTRING`, `LENGTH`, `UPPER`, `LOWER`, `LEFT`

## Tools Used

- MySQL Workbench
