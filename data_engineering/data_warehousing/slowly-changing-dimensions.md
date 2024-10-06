### What are Slowly Changing Dimensions (SCD)?

**Slowly Changing Dimensions (SCD)** are a concept in data warehousing where the attributes of a dimension (such as a customer, product, or employee) change slowly over time. A dimension represents descriptive or contextual information that typically does not change often, but when it does, the data warehouse needs a strategy to handle these changes while maintaining historical data integrity.

In essence, SCDs manage how to track and store both the current and historical data for dimension attributes when changes occur. For example, if a customer changes their address, the data warehouse needs to determine whether to overwrite the old address, keep the old address for historical records, or handle the change in some other way.

---

### Types of Slowly Changing Dimensions (SCD)

There are several methods for handling SCDs, commonly classified into **SCD Types**. Each type defines a different strategy for managing changes to dimension data.

#### 1. **Type 0 – Retain Original**

In **SCD Type 0**, no changes are made to the original data. Once the data is inserted, it remains unchanged forever. This type is useful when the data is considered static and historical data should never be updated or modified.

- **Example**: For certain product codes or employee IDs that should never change, once recorded, the dimension attributes remain fixed.

#### 2. **Type 1 – Overwrite**

In **SCD Type 1**, the current value of a dimension is simply **overwritten** when an attribute changes. No historical data is retained, and only the latest value is available in the data warehouse.

- **Use Case**: When historical values are not important, and only the most recent value is relevant.
  
- **Example**: If a customer changes their phone number, the old phone number is replaced by the new one in the dimension table.

```sql
UPDATE customer_dimension
SET phone_number = '123-456-7890'
WHERE customer_id = 101;
```

**Pros**:
- Simple to implement.
- Saves storage space since only the latest value is stored.

**Cons**:
- Historical data is lost, so past trends and analytics on previous values are not possible.

---

#### 3. **Type 2 – Add New Row (Versioning)**

In **SCD Type 2**, historical data is retained by adding a new row for each change to a dimension attribute. Each version of the dimension is associated with specific time ranges (e.g., start and end dates) or with a version number.

- **Use Case**: When it is important to track changes over time and retain the history of attribute values.

- **Example**: If a customer changes their address, a new row is added with the updated address, and the previous row is closed by marking the `end_date` or incrementing a version number.

```sql
-- Mark the old record as expired
UPDATE customer_dimension
SET end_date = CURRENT_DATE
WHERE customer_id = 101 AND end_date IS NULL;

-- Insert a new record with the updated address and a new start_date
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date)
VALUES (101, 'John Doe', 'New Address', CURRENT_DATE, NULL);
```

**Pros**:
- Historical data is preserved, allowing for trend analysis and tracking changes over time.
- Supports reporting based on past data.

**Cons**:
- Increases storage space as new rows are added for each change.
- Queries may become more complex, as they need to filter by the current or relevant version of the data.

---

#### 4. **Type 3 – Add New Column**

In **SCD Type 3**, a new column is added to store the **previous value** of the attribute. This approach allows tracking one level of historical changes, but it does not maintain a full history.

- **Use Case**: When only the previous value of an attribute is relevant for reporting or analysis, but complete historical tracking is not needed.

- **Example**: If a customer changes their email address, the current email is stored in one column, and the previous email is stored in a separate column.

```sql
-- Update the current email, but keep the previous email in a separate column
UPDATE customer_dimension
SET previous_email = email,
    email = 'newemail@example.com'
WHERE customer_id = 101;
```

**Pros**:
- Easy to implement and query.
- Allows for limited historical tracking without consuming too much space.

**Cons**:
- Can only track one previous value; additional historical changes are not tracked.

---

#### 5. **Type 4 – Add Historical Table**

In **SCD Type 4**, a separate **historical table** is maintained for tracking changes to dimension attributes. The current version of the dimension is kept in one table, while all historical versions are stored in a separate table.

- **Use Case**: When it is necessary to keep a clean, optimized table for current values, while also maintaining a complete history in a separate table.

- **Example**: A customer dimension table holds only the current address, while a separate history table stores all previous addresses with timestamps.

```sql
-- Update the current table
UPDATE customer_dimension
SET address = 'New Address'
WHERE customer_id = 101;

-- Insert a record into the history table
INSERT INTO customer_dimension_history (customer_id, address, change_date)
VALUES (101, 'Old Address', CURRENT_DATE);
```

**Pros**:
- Keeps the current dimension table small and optimized for query performance.
- Full history is maintained in a separate table, enabling historical analysis.

**Cons**:
- Requires managing two tables.
- Queries for historical data require joining with the historical table.

---

#### 6. **Type 6 – Hybrid SCD (Combining Type 1, 2, and 3)**

**SCD Type 6** (also known as a **hybrid approach**) combines aspects of SCD Types 1, 2, and 3. It includes adding a new row for each change (like Type 2), maintaining previous values in separate columns (like Type 3), and allowing overwrites of some attributes (like Type 1). This method provides flexibility in handling different attributes with different strategies.

- **Use Case**: When a mix of tracking current values, previous values, and complete history is required for different attributes within the same dimension.

- **Example**: A customer dimension may track the most recent and previous email addresses (Type 3), add new rows for changes in customer address (Type 2), and overwrite some attributes like phone number (Type 1).

```sql
-- Insert a new row for address change
INSERT INTO customer_dimension (customer_id, customer_name, address, previous_email, email, start_date, end_date)
VALUES (101, 'John Doe', 'New Address', 'oldemail@example.com', 'newemail@example.com', CURRENT_DATE, NULL);

-- Overwrite the phone number without tracking history
UPDATE customer_dimension
SET phone_number = '123-456-7890'
WHERE customer_id = 101;
```

**Pros**:
- Provides flexibility in how different attributes are handled, allowing for mixed historical tracking.
- Can minimize the need for complex queries while preserving key historical data.

**Cons**:
- Complexity in implementation and maintenance due to the hybrid nature.
- May consume more storage and require more intricate ETL logic.

---

### Choosing the Right SCD Type

The choice of SCD type depends on the business requirements and how important it is to track the history of data changes. Here are some factors to consider:

1. **Need for Historical Data**: If it's important to maintain a complete history of changes, **SCD Type 2** is typically the best choice.
2. **Storage Constraints**: If storage is a concern and history is not required, **SCD Type 1** (overwrite) can be more efficient.
3. **Limited Historical Requirements**: If only the previous value of an attribute is needed, **SCD Type 3** provides a simple solution without adding new rows.
4. **Query Complexity**: **SCD Type 4** helps keep the main dimension table simple by moving historical data to a separate table.
5. **Hybrid Needs**: If different attributes require different handling, **SCD Type 6** can provide a flexible hybrid solution.

---

### Handling SCD in ETL Processes

The **ETL (Extract, Transform, Load)** process plays a key role in implementing SCD strategies. The ETL workflow needs to be designed to:

- **Detect Changes**: ETL processes should detect changes in dimension attributes, typically using techniques like **Change Data Capture (CDC)**, timestamps, or versioning.
  
- **Insert or Update**: Based on the SCD type being used, the ETL process should either overwrite the existing data (Type 1), add a new row (Type 2), or update specific columns (Type 3).

- **Data Validation**: Ensure that the changes are valid and meet business rules (e.g., ensuring that valid date ranges or null checks are applied).

- **Maintain Metadata**: Track metadata such as `start_date`, `end_date`, or version numbers to correctly capture the timeline of changes.

---

### Conclusion

**Slowly Changing Dimensions (SCD)** are a vital aspect of data warehouse design, ensuring that historical changes to dimension attributes are managed properly. Different SCD types (Type 1, Type 2, Type 3, etc.) provide different strategies for handling changes based on the business needs for tracking historical data and the importance of past values. The right SCD approach should align with the organization’s reporting requirements, storage capacity, and complexity tolerance, with the ETL process designed to implement and manage these changes effectively.