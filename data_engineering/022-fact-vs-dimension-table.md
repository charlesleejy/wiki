## 22. Explain the concept of a fact table and a dimension table.


### Explain the Concept of a Fact Table and a Dimension Table

#### Fact Table

1. **Definition**:
   - A fact table is the central table in a star or snowflake schema in a data warehouse. It contains quantitative data (facts) for analysis and measurement.

2. **Characteristics**:
   - **Quantitative Data**: Stores numerical measures that can be aggregated (e.g., sales, revenue, quantity).
   - **Foreign Keys**: Contains foreign keys that reference primary keys in the dimension tables.
   - **Granularity**: The level of detail stored in the fact table (e.g., daily sales, monthly revenue).

3. **Components**:
   - **Measures**: Quantitative data used for analysis (e.g., total_sales, quantity_sold).
   - **Foreign Keys**: Keys that link to dimension tables (e.g., date_id, product_id, customer_id).

4. **Example**:
   ```sql
   CREATE TABLE SalesFact (
       sale_id INT PRIMARY KEY,
       date_id INT,
       product_id INT,
       customer_id INT,
       store_id INT,
       total_sales DECIMAL(10, 2),
       quantity_sold INT,
       FOREIGN KEY (date_id) REFERENCES DateDimension(date_id),
       FOREIGN KEY (product_id) REFERENCES ProductDimension(product_id),
       FOREIGN KEY (customer_id) REFERENCES CustomerDimension(customer_id),
       FOREIGN KEY (store_id) REFERENCES StoreDimension(store_id)
   );
   ```

5. **Types of Fact Tables**:
   - **Transactional Fact Table**: Records individual transactions.
   - **Snapshot Fact Table**: Captures data at specific points in time.
   - **Aggregated Fact Table**: Stores pre-aggregated data for faster query performance.

6. **Use Case**:
   - **Sales Analysis**: Analyze total sales, quantities sold, and revenue over different periods and across various products, customers, and stores.

#### Dimension Table

1. **Definition**:
   - A dimension table is a table in a star or snowflake schema that contains descriptive attributes (dimensions) related to the facts in the fact table.

2. **Characteristics**:
   - **Descriptive Data**: Stores textual or categorical data (e.g., product name, customer name, date).
   - **Primary Keys**: Contains primary keys that uniquely identify each dimension record.
   - **Denormalized**: Typically denormalized to improve query performance, though can be normalized in snowflake schema.

3. **Components**:
   - **Attributes**: Descriptive information about dimensions (e.g., product_name, category, customer_name, region).
   - **Primary Key**: Unique identifier for each record in the dimension table (e.g., product_id, customer_id).

4. **Example**:
   ```sql
   CREATE TABLE ProductDimension (
       product_id INT PRIMARY KEY,
       product_name VARCHAR(50),
       category VARCHAR(50)
   );

   CREATE TABLE CustomerDimension (
       customer_id INT PRIMARY KEY,
       customer_name VARCHAR(50),
       region VARCHAR(50)
   );

   CREATE TABLE DateDimension (
       date_id INT PRIMARY KEY,
       date DATE,
       month VARCHAR(20),
       quarter VARCHAR(20),
       year INT
   );

   CREATE TABLE StoreDimension (
       store_id INT PRIMARY KEY,
       store_name VARCHAR(50),
       location VARCHAR(50)
   );
   ```

5. **Use Case**:
   - **Contextual Analysis**: Provide context and detailed information for analysis. For instance, understanding which products sold the most, which regions have the highest sales, and during which periods sales peaked.

#### Differences Between Fact Table and Dimension Table

1. **Data Type**:
   - **Fact Table**: Contains quantitative data (measures).
   - **Dimension Table**: Contains descriptive data (attributes).

2. **Purpose**:
   - **Fact Table**: Used for storing measurable, quantitative data for analysis.
   - **Dimension Table**: Used for storing descriptive information to provide context to the facts.

3. **Key Relationships**:
   - **Fact Table**: Contains foreign keys that reference dimension tables.
   - **Dimension Table**: Contains primary keys that are referenced by fact tables.

4. **Granularity**:
   - **Fact Table**: Defines the level of detail (e.g., individual transactions, daily snapshots).
   - **Dimension Table**: Defines the descriptive detail related to the facts (e.g., product details, customer information).

5. **Schema**:
   - **Fact Table**: Central table in a star or snowflake schema.
   - **Dimension Table**: Surrounding tables in a star or snowflake schema.

#### Example Schema with Fact and Dimension Tables

1. **Sales Fact Table**:
   - Measures: total_sales, quantity_sold
   - Foreign Keys: date_id, product_id, customer_id, store_id

2. **Product Dimension Table**:
   - Attributes: product_id, product_name, category

3. **Customer Dimension Table**:
   - Attributes: customer_id, customer_name, region

4. **Date Dimension Table**:
   - Attributes: date_id, date, month, quarter, year

5. **Store Dimension Table**:
   - Attributes: store_id, store_name, location

### Summary

- **Fact Table**:
  - Contains quantitative data for analysis (measures).
  - Central table in the schema with foreign keys to dimension tables.
  - Used for storing and analyzing metrics like sales, revenue, and quantities.

- **Dimension Table**:
  - Contains descriptive attributes related to facts (dimensions).
  - Surrounding tables in the schema with primary keys.
  - Used for providing context and detailed information to the facts.

Understanding the roles of fact and dimension tables is crucial for designing efficient and effective data warehouse schemas that support complex queries and data analysis.