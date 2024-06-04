# Normalization in DBMS

Normalization in DBMS (Database Management System) is a process of organizing data in a database to reduce redundancy and improve data integrity. The main goal of normalization is to divide large tables into smaller ones and define relationships between them to make the database more efficient. The process involves several normal forms, each with specific rules and requirements.

## Key Concepts of Normalization

1. **Functional Dependency**: A relationship between two attributes, typically between a primary key and a non-key attribute. For example, in a table with attributes A and B, B is functionally dependent on A if each value of A is associated with exactly one value of B.

2. **Primary Key**: A unique identifier for a record in a table. It ensures that each record can be uniquely identified.

### Functional Dependency Example

Functional dependencies describe a relationship between attributes in a database table. A functional dependency ( A -> B) means that attribute B is functionally dependent on attribute A if each value of A is associated with exactly one value of B.

**Example Scenario**

Consider a table of employee information:

**Initial Table**:

| EmployeeID | EmployeeName | DepartmentID | DepartmentName |
|------------|--------------|--------------|----------------|
| 1          | Alice        | 101          | HR             |
| 2          | Bob          | 102          | IT             |
| 3          | Charlie      | 101          | HR             |
| 4          | David        | 103          | Finance        |

### Functional Dependencies

1. **EmployeeID -> EmployeeName**: Each employee ID uniquely determines an employee name.
2. **DepartmentID -> DepartmentName**: Each department ID uniquely determines a department name.

These functional dependencies can be written as:

- EmployeeID -> EmployeeName 
- DepartmentID -> DepartmentName 

**Explanation**:

- For every unique EmployeeID, there is a unique EmployeeName. This means if you know the EmployeeID, you can determine the EmployeeName.
- Similarly, for every unique DepartmentID, there is a unique DepartmentName. Knowing the DepartmentID allows you to determine the DepartmentName.

By identifying these functional dependencies, you can design your database schema to ensure data integrity and reduce redundancy.


## Normal Forms

Normalization typically involves applying the following normal forms, each addressing specific types of anomalies:

### First Normal Form (1NF)

- Ensures that the values in each column of a table are atomic (indivisible).
- Removes repeating groups of columns.

**Example**: A table with multiple phone numbers in a single column needs to be split so that each phone number is in its own column or record.

**Before 1NF**:

| ID | Name   | Phone Numbers       |
|----|--------|---------------------|
| 1  | Alice  | 12345, 67890        |
| 2  | Bob    | 54321, 09876        |

**After 1NF**:

| ID | Name   | Phone Number |
|----|--------|--------------|
| 1  | Alice  | 12345        |
| 1  | Alice  | 67890        |
| 2  | Bob    | 54321        |
| 2  | Bob    | 09876        |

### Second Normal Form (2NF)

- Ensures that the table is in 1NF.
- Removes partial dependencies; i.e., non-key attributes should depend on the whole primary key, not just part of it.

**Example**: If a table has a composite primary key, make sure that non-key attributes are dependent on all parts of the key.

**Before 2NF**:

| StudentID | CourseID | InstructorName | Grade |
|-----------|----------|----------------|-------|
| 1         | 101      | Smith          | A     |
| 2         | 102      | Jones          | B     |

**After 2NF**:
- Decompose into two tables:

**Students_Courses**:

| StudentID | CourseID | Grade |
|-----------|----------|-------|
| 1         | 101      | A     |
| 2         | 102      | B     |

**Courses**:

| CourseID | InstructorName |
|----------|----------------|
| 101      | Smith          |
| 102      | Jones          |

### Third Normal Form (3NF)

- Ensures that the table is in 2NF.
- Removes transitive dependencies; i.e., non-key attributes should depend only on the primary key and not on other non-key attributes.

**Example**: If an attribute depends on another non-key attribute, separate them into different tables.

**Before 3NF**:

| StudentID | CourseID | InstructorID | InstructorName |
|-----------|----------|--------------|----------------|
| 1         | 101      | 201          | Smith          |
| 2         | 102      | 202          | Jones          |

**After 3NF**:
- Decompose into three tables:

**Students_Courses**:

| StudentID | CourseID | InstructorID |
|-----------|----------|--------------|
| 1         | 101      | 201          |
| 2         | 102      | 202          |

**Instructors**:

| InstructorID | InstructorName |
|--------------|----------------|
| 201          | Smith          |
| 202          | Jones          |

### Boyce-Codd Normal Form (BCNF)

BCNF is a stricter version of the Third Normal Form (3NF). A table is in BCNF if it is in 3NF and, for every non-trivial functional dependency \( A -> B \), \( A \) is a superkey. A superkey is a set of one or more columns that can uniquely identify a row in a table.

#### Example Scenario

Consider a scenario where a university database tracks courses, instructors, and the rooms where classes are held.

**Initial Table (1NF)**:

| CourseID | Instructor | Room        |
|----------|------------|-------------|
| 101      | Dr. Smith  | Room 1      |
| 102      | Dr. Johnson| Room 2      |
| 101      | Dr. Smith  | Room 3      |
| 103      | Dr. White  | Room 1      |

### Second Normal Form (2NF)

The table is already in 2NF because it has no partial dependencies, given that the primary key is the composite key (CourseID, Instructor, Room), and non-key attributes are functionally dependent on the entire primary key.

### Third Normal Form (3NF)

The table is already in 3NF because it has no transitive dependencies.

### Boyce-Codd Normal Form (BCNF)

To achieve BCNF, we need to ensure that for every non-trivial functional dependency \( A -> B \), \( A \) must be a superkey.

In the given table, we have the following functional dependencies:

1. **CourseID, Instructor -> Room**: This means that for a given course and instructor, a specific room is assigned.
2. **Room -> Instructor**: This implies that a room can only be assigned to one instructor at a time.

The second functional dependency \( Room -> Instructor \) violates BCNF because Room is not a superkey, but it determines Instructor. To fix this, we decompose the table.

**Decomposed Tables for BCNF**:

1. **Courses_Rooms**:

| CourseID | Room  |
|----------|-------|
| 101      | Room 1|
| 101      | Room 3|
| 102      | Room 2|
| 103      | Room 1|

2. **Rooms_Instructors**:

| Room  | Instructor |
|-------|------------|
| Room 1| Dr. Smith  |
| Room 2| Dr. Johnson|
| Room 3| Dr. Smith  |
| Room 1| Dr. White  |

## Benefits of Normalization

- **Reduces Data Redundancy**: By eliminating duplicate data, the database becomes more efficient.
- **Improves Data Integrity**: Ensures that data dependencies are logical, making updates and deletions less error-prone.
- **Optimizes Query Performance**: Smaller, well-structured tables can improve query performance.

## Trade-offs

- **Complexity**: Highly normalized databases can be complex and harder to design.
- **Performance**: Excessive normalization might lead to more joins, which can impact performance.

## Summary

This example demonstrates how normalization breaks down a table into smaller, more manageable pieces, ensuring data integrity and reducing redundancy. Each normal form addresses specific types of anomalies, progressively making the database more efficient and less prone to errors.
