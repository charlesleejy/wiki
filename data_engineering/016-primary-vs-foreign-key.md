## 16. What is a primary key, and how does it differ from a foreign key?


### What is a Primary Key, and How Does it Differ from a Foreign Key?

#### Primary Key

1. **Definition**:
   - A primary key is a column or a set of columns in a table that uniquely identifies each row in that table. It ensures that no two rows have the same primary key value, providing a unique identifier for each record.

2. **Characteristics**:
   - **Uniqueness**: Each value in the primary key column(s) must be unique.
   - **Non-null**: A primary key cannot contain NULL values.
   - **Immutability**: The values of primary key columns should not change. They remain constant for the lifetime of the record.
   - **Single Column or Composite**: A primary key can be a single column or a combination of multiple columns (composite key).

3. **Purpose**:
   - **Unique Identification**: Ensures that each record can be uniquely identified in the table.
   - **Indexing**: Automatically indexed by the database to improve query performance.
   - **Referential Integrity**: Used to establish and enforce relationships between tables.

4. **Example**:
   ```sql
   CREATE TABLE Employees (
       employee_id INT PRIMARY KEY,
       name VARCHAR(100),
       department_id INT
   );
   ```

#### Foreign Key

1. **Definition**:
   - A foreign key is a column or a set of columns in a table that creates a link between data in two tables. It is a reference to the primary key in another table, establishing a relationship between the tables.

2. **Characteristics**:
   - **Referential Integrity**: Ensures that the value in the foreign key column matches a value in the referenced primary key column or is NULL.
   - **Can Be NULL**: Unlike primary keys, foreign keys can contain NULL values.
   - **Multiple Foreign Keys**: A table can have multiple foreign keys, each pointing to a primary key in different tables.

3. **Purpose**:
   - **Relationship Definition**: Defines and enforces relationships between tables, maintaining the integrity of the data.
   - **Data Consistency**: Ensures that relationships between tables remain consistent (e.g., you cannot have a child record without a corresponding parent record).

4. **Example**:
   ```sql
   CREATE TABLE Departments (
       department_id INT PRIMARY KEY,
       department_name VARCHAR(100)
   );

   CREATE TABLE Employees (
       employee_id INT PRIMARY KEY,
       name VARCHAR(100),
       department_id INT,
       FOREIGN KEY (department_id) REFERENCES Departments(department_id)
   );
   ```

### Differences Between Primary Key and Foreign Key

1. **Purpose**:
   - **Primary Key**: Uniquely identifies each record in a table.
   - **Foreign Key**: Establishes a link between records in two tables, maintaining referential integrity.

2. **Uniqueness**:
   - **Primary Key**: Must be unique across the table.
   - **Foreign Key**: Can have duplicate values because it references primary key values in another table.

3. **Nullability**:
   - **Primary Key**: Cannot contain NULL values.
   - **Foreign Key**: Can contain NULL values if the relationship is optional.

4. **Number of Keys**:
   - **Primary Key**: Each table can have only one primary key.
   - **Foreign Key**: A table can have multiple foreign keys.

5. **Indexing**:
   - **Primary Key**: Automatically indexed to ensure quick data retrieval.
   - **Foreign Key**: Not automatically indexed, but indexing is often recommended for performance reasons.

6. **Data Relationship**:
   - **Primary Key**: Defines the unique identifier for records within its own table.
   - **Foreign Key**: Defines the relationship between records in different tables.

### Summary
- **Primary Key**: A unique identifier for each record in a table, ensuring that no two rows have the same primary key value. It cannot be NULL and is automatically indexed by the database.
- **Foreign Key**: A column or a set of columns that establishes a link between data in two tables, referencing the primary key of another table. It can contain NULL values and ensures referential integrity between the tables.
- **Differences**: Include their purpose, uniqueness, nullability, number of keys allowed per table, indexing, and their role in data relationships.

Understanding the roles of primary keys and foreign keys is fundamental to designing relational databases that are both efficient and maintain data integrity.