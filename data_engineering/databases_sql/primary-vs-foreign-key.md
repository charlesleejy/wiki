### What is a Primary Key?

A **primary key** is a column (or a combination of columns) in a database table that uniquely identifies each row in that table. It serves as a unique identifier for records, ensuring that no two rows in the table have the same primary key value. Every table can have **only one primary key**, and the primary key must follow certain rules:

#### Key Characteristics of a Primary Key:
1. **Uniqueness**: Each value in the primary key column must be unique. This guarantees that each row can be uniquely identified.
2. **Non-nullability**: The primary key column(s) cannot contain `NULL` values because every row must have a valid, unique identifier.
3. **Single Per Table**: There can only be one primary key for each table, though it can be made up of multiple columns (called a **composite primary key**).

#### Example:
Consider an `employees` table, where `employee_id` is the primary key:
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department VARCHAR(50)
);
```
In this case, `employee_id` is the primary key and ensures that each employee has a unique identifier.

---

### What is a Foreign Key?

A **foreign key** is a column (or a combination of columns) in one table that establishes a link between the data in two tables. It **references** the primary key or a unique key of another table, creating a relationship between the two tables. The foreign key ensures that the values in the foreign key column(s) correspond to valid values in the referenced table's primary or unique key column(s).

#### Key Characteristics of a Foreign Key:
1. **Referential Integrity**: The foreign key enforces a relationship between the two tables by ensuring that a record cannot exist in the child table unless a corresponding record exists in the parent table.
2. **Allows Duplicate and Null Values**: Unlike primary keys, foreign keys can contain duplicate values and may allow `NULL` values (unless explicitly restricted).
3. **Multiple Foreign Keys Per Table**: A table can have multiple foreign keys, referencing primary keys in different tables.

#### Example:
Consider an `orders` table where the `customer_id` is a foreign key that references the `customer_id` column in the `customers` table:
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
In this example:
- `customer_id` in the `orders` table is a foreign key that references the `customer_id` in the `customers` table. This ensures that an order is always associated with an existing customer.

---

### Key Differences Between Primary Key and Foreign Key

| **Aspect**                  | **Primary Key**                                    | **Foreign Key**                                  |
|-----------------------------|----------------------------------------------------|--------------------------------------------------|
| **Purpose**                  | Uniquely identifies each row within its own table. | Establishes a relationship between two tables by referencing the primary key of another table. |
| **Uniqueness**               | Must be unique across all rows.                    | Can have duplicate values, as it represents a reference to another table. |
| **Nullability**              | Cannot contain `NULL` values.                      | Can contain `NULL` values unless otherwise restricted. |
| **Number of Keys Per Table** | Only one primary key per table.                    | A table can have multiple foreign keys.          |
| **Enforces**                 | Entity integrity (ensures that each row is unique and identifiable). | Referential integrity (ensures that records in one table are linked to valid records in another). |
| **Scope**                    | Used within the same table to uniquely identify rows. | Used to link two tables, typically between a child and a parent table. |
| **Creation**                 | Defined using the `PRIMARY KEY` constraint.        | Defined using the `FOREIGN KEY` constraint.      |

---

### Example of a Relationship Between Primary Key and Foreign Key

Consider a **Customer** and **Order** relationship in an e-commerce system. The `customers` table has a primary key (`customer_id`), and the `orders` table uses a foreign key (`customer_id`) to reference the `customers` table.

#### customers Table:
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);
```
- `customer_id` is the primary key and uniquely identifies each customer.

#### orders Table:
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
- `customer_id` in the `orders` table is a foreign key that references the `customer_id` in the `customers` table. This enforces the rule that no order can exist without a valid customer in the `customers` table.

---

### Use Cases for Primary and Foreign Keys

#### Primary Key:
- **Use Case**: The primary key is used to uniquely identify each row within a table. For example, in a `students` table, `student_id` could be a primary key that uniquely identifies each student.
  
#### Foreign Key:
- **Use Case**: Foreign keys establish relationships between tables. For example, in a `course_enrollment` table, `student_id` might be a foreign key that references the `students` table, ensuring that no enrollment record exists without a corresponding student.

---

### Referential Integrity and Foreign Keys

Foreign keys are crucial for maintaining **referential integrity** in a database. Referential integrity ensures that relationships between tables remain consistent. For example:
- If you try to insert a row into the `orders` table with a `customer_id` that doesn't exist in the `customers` table, the foreign key constraint would prevent this, ensuring data integrity.
- If you try to delete a customer that has associated orders, the database will prevent the deletion (unless you specify **ON DELETE CASCADE**), ensuring that no orphaned orders exist.

---

### Conclusion

- A **primary key** uniquely identifies each record in a table and ensures that no two rows can have the same value in the primary key column(s). It cannot be `NULL` and enforces **entity integrity** within a single table.
- A **foreign key** is used to establish a link between two tables by referencing the primary key of another table, ensuring **referential integrity** across the tables.

Together, primary and foreign keys help maintain data integrity, consistency, and relationships in a relational database.