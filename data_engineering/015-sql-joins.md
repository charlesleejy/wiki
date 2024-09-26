## 15. Explain the different types of joins in SQL.


### Explain the Different Types of Joins in SQL

#### 1. INNER JOIN

- **Definition**: Returns only the rows where there is a match in both tables.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  INNER JOIN table2
  ON table1.column = table2.column;
  ```
- **Example**:
  ```sql
  SELECT Employees.name, Departments.department_name
  FROM Employees
  INNER JOIN Departments
  ON Employees.department_id = Departments.department_id;
  ```
- **Use Case**: Used when you want to retrieve records that have matching values in both tables.

#### 2. LEFT JOIN (or LEFT OUTER JOIN)

- **Definition**: Returns all rows from the left table and the matched rows from the right table. If there is no match, NULL values are returned for columns from the right table.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  LEFT JOIN table2
  ON table1.column = table2.column;
  ```
- **Example**:
  ```sql
  SELECT Employees.name, Departments.department_name
  FROM Employees
  LEFT JOIN Departments
  ON Employees.department_id = Departments.department_id;
  ```
- **Use Case**: Used when you need all records from the left table, regardless of whether they have a match in the right table.

#### 3. RIGHT JOIN (or RIGHT OUTER JOIN)

- **Definition**: Returns all rows from the right table and the matched rows from the left table. If there is no match, NULL values are returned for columns from the left table.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  RIGHT JOIN table2
  ON table1.column = table2.column;
  ```
- **Example**:
  ```sql
  SELECT Employees.name, Departments.department_name
  FROM Employees
  RIGHT JOIN Departments
  ON Employees.department_id = Departments.department_id;
  ```
- **Use Case**: Used when you need all records from the right table, regardless of whether they have a match in the left table.

#### 4. FULL JOIN (or FULL OUTER JOIN)

- **Definition**: Returns rows when there is a match in one of the tables. This means it returns all records when there is a match in either left or right table. If there is no match, the result is NULL on the side where there is no match.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  FULL JOIN table2
  ON table1.column = table2.column;
  ```
- **Example**:
  ```sql
  SELECT Employees.name, Departments.department_name
  FROM Employees
  FULL JOIN Departments
  ON Employees.department_id = Departments.department_id;
  ```
- **Use Case**: Used when you need all records from both tables, with matching records from both sides and NULLs where there is no match.

#### 5. CROSS JOIN

- **Definition**: Returns the Cartesian product of the two tables, i.e., all possible combinations of rows.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  CROSS JOIN table2;
  ```
- **Example**:
  ```sql
  SELECT Employees.name, Departments.department_name
  FROM Employees
  CROSS JOIN Departments;
  ```
- **Use Case**: Used when you need to combine all rows from two tables, often for generating all combinations or testing.

#### 6. SELF JOIN

- **Definition**: A regular join but the table is joined with itself.
- **Syntax**:
  ```sql
  SELECT a.columns, b.columns
  FROM table a, table b
  WHERE a.column = b.column;
  ```
- **Example**:
  ```sql
  SELECT e1.name AS Employee, e2.name AS Manager
  FROM Employees e1
  INNER JOIN Employees e2
  ON e1.manager_id = e2.employee_id;
  ```
- **Use Case**: Used to combine rows with other rows in the same table, often to compare rows within the same table.

#### 7. NATURAL JOIN

- **Definition**: A type of join that is based on columns with the same name and compatible data types in both tables.
- **Syntax**:
  ```sql
  SELECT columns
  FROM table1
  NATURAL JOIN table2;
  ```
- **Example**:
  ```sql
  SELECT *
  FROM Employees
  NATURAL JOIN Departments;
  ```
- **Use Case**: Simplifies the join process by automatically determining the columns to join on based on the same name and data type.

### Summary
- **INNER JOIN**: Returns only the matching rows between two tables.
- **LEFT JOIN**: Returns all rows from the left table and matched rows from the right table.
- **RIGHT JOIN**: Returns all rows from the right table and matched rows from the left table.
- **FULL JOIN**: Returns rows when there is a match in one of the tables, including all rows from both tables.
- **CROSS JOIN**: Returns the Cartesian product of the two tables.
- **SELF JOIN**: Joins a table with itself.
- **NATURAL JOIN**: Automatically joins tables based on columns with the same name and compatible data types.

Understanding the different types of joins in SQL is crucial for efficiently querying relational databases and retrieving the desired data accurately. Each join type serves a specific purpose and is suited to different types of queries and data relationships.