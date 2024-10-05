### What Are Window Functions in SQL?

**Window functions** in SQL are advanced functions that perform calculations across a set of table rows that are related to the current row. Unlike aggregate functions (like `SUM`, `COUNT`, or `AVG`), which return a single result for a group of rows, window functions retain the individual rows while computing a result for each row based on a defined "window" of related rows.

Window functions are often used for performing complex calculations such as running totals, moving averages, ranking, and cumulative sums. They are powerful tools for analytical queries where multiple rows are compared or analyzed in relation to each other.

---

### Syntax of Window Functions

Window functions use the `OVER()` clause, which defines the "window" (or subset of rows) over which the function is calculated. The basic syntax is:

```sql
<window_function>() OVER ( [PARTITION BY <expression>] [ORDER BY <expression>] [ROWS | RANGE BETWEEN ...] )
```

- **PARTITION BY**: Divides the result set into partitions or groups, similar to the `GROUP BY` clause, but without collapsing the rows. The window function is applied separately within each partition.
- **ORDER BY**: Specifies the order of rows within the window for calculations like running totals or rankings.
- **ROWS | RANGE**: Specifies how the window frame (set of rows) is determined relative to the current row (used for more complex windowing logic).

---

### Types of Window Functions

1. **Aggregate Window Functions**:
   - **SUM()**: Calculates the sum of values over the window.
   - **AVG()**: Calculates the average of values over the window.
   - **MIN()** / **MAX()**: Finds the minimum or maximum value within the window.

2. **Ranking Window Functions**:
   - **ROW_NUMBER()**: Assigns a unique sequential integer to rows within the window.
   - **RANK()**: Assigns a rank to each row, with ties receiving the same rank and leaving gaps.
   - **DENSE_RANK()**: Similar to `RANK()`, but without gaps in ranking when there are ties.
   - **NTILE(n)**: Divides the result set into `n` roughly equal parts and assigns a number to each part.

3. **Value Window Functions**:
   - **LEAD()**: Returns the value from the next row in the window.
   - **LAG()**: Returns the value from the previous row in the window.
   - **FIRST_VALUE()**: Returns the first value in the window.
   - **LAST_VALUE()**: Returns the last value in the window.
   - **NTH_VALUE()**: Returns the nth value in the window.

---

### Examples of Using Window Functions

Let’s consider an `orders` table with the following columns: `order_id`, `customer_id`, `order_date`, and `order_amount`.

| order_id | customer_id | order_date | order_amount |
|----------|-------------|------------|--------------|
| 1        | 101         | 2023-01-10 | 100          |
| 2        | 102         | 2023-01-11 | 200          |
| 3        | 101         | 2023-01-12 | 150          |
| 4        | 103         | 2023-01-12 | 300          |
| 5        | 102         | 2023-01-13 | 250          |

---

### 1. **ROW_NUMBER()**: Assign a Row Number to Each Row

You can use the `ROW_NUMBER()` function to assign a sequential number to each row within a specific partition (or across the entire table).

**Example**: Assign row numbers to each order, ordered by `order_date`:
```sql
SELECT order_id, customer_id, order_amount, 
       ROW_NUMBER() OVER (ORDER BY order_date) AS row_num
FROM orders;
```

**Result**:
| order_id | customer_id | order_amount | row_num |
|----------|-------------|--------------|---------|
| 1        | 101         | 100          | 1       |
| 2        | 102         | 200          | 2       |
| 3        | 101         | 150          | 3       |
| 4        | 103         | 300          | 4       |
| 5        | 102         | 250          | 5       |

---

### 2. **RANK()**: Rank Orders by Amount

You can use the `RANK()` function to assign a ranking to each row based on the `order_amount`. If there are ties, the same rank is given, and gaps are left in the ranking sequence.

**Example**: Rank orders by `order_amount`:
```sql
SELECT order_id, customer_id, order_amount, 
       RANK() OVER (ORDER BY order_amount DESC) AS order_rank
FROM orders;
```

**Result**:
| order_id | customer_id | order_amount | order_rank |
|----------|-------------|--------------|------------|
| 4        | 103         | 300          | 1          |
| 5        | 102         | 250          | 2          |
| 2        | 102         | 200          | 3          |
| 3        | 101         | 150          | 4          |
| 1        | 101         | 100          | 5          |

---

### 3. **LEAD() and LAG()**: Access Previous or Next Row Values

You can use `LEAD()` and `LAG()` to access values from previous or next rows within the same window, useful for comparing values between rows.

**Example**: Find the next order amount for each customer:
```sql
SELECT order_id, customer_id, order_amount, 
       LEAD(order_amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_order_amount
FROM orders;
```

**Result**:
| order_id | customer_id | order_amount | next_order_amount |
|----------|-------------|--------------|-------------------|
| 1        | 101         | 100          | 150               |
| 3        | 101         | 150          | NULL              |
| 2        | 102         | 200          | 250               |
| 5        | 102         | 250          | NULL              |
| 4        | 103         | 300          | NULL              |

---

### 4. **SUM()**: Calculate Running Totals

The `SUM()` window function can be used to calculate running totals, i.e., the cumulative sum of values over the window.

**Example**: Calculate a running total of `order_amount` by `customer_id`:
```sql
SELECT order_id, customer_id, order_amount, 
       SUM(order_amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total
FROM orders;
```

**Result**:
| order_id | customer_id | order_amount | running_total |
|----------|-------------|--------------|---------------|
| 1        | 101         | 100          | 100           |
| 3        | 101         | 150          | 250           |
| 2        | 102         | 200          | 200           |
| 5        | 102         | 250          | 450           |
| 4        | 103         | 300          | 300           |

---

### 5. **NTILE()**: Distribute Rows into Buckets

`NTILE()` divides the result set into `n` buckets (or roughly equal parts) and assigns a bucket number to each row.

**Example**: Divide orders into two buckets based on `order_date`:
```sql
SELECT order_id, customer_id, order_date, 
       NTILE(2) OVER (ORDER BY order_date) AS bucket
FROM orders;
```

**Result**:
| order_id | customer_id | order_date | bucket |
|----------|-------------|------------|--------|
| 1        | 101         | 2023-01-10 | 1      |
| 2        | 102         | 2023-01-11 | 1      |
| 3        | 101         | 2023-01-12 | 2      |
| 4        | 103         | 2023-01-12 | 2      |
| 5        | 102         | 2023-01-13 | 2      |

---

### Use Cases of Window Functions

- **Ranking and Top-N Queries**: Window functions like `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` are useful when you need to assign ranks or find the top N records within a partition.
- **Running Totals and Moving Averages**: Aggregate window functions like `SUM()`, `AVG()`, and `COUNT()` help in calculating running totals, cumulative sums, or moving averages over a window of data.
- **Comparing Values Across Rows**: Functions like `LAG()` and `LEAD()` allow you to compare values from preceding or succeeding rows, which is useful for time-series analysis, trend identification, or sequence comparison.
- **Bucketing Data**: `NTILE()` helps divide data into groups, which is useful in creating percentiles or distributing workloads.

---

### Conclusion

Window functions in SQL are powerful tools for performing advanced calculations across rows related to the current row, without collapsing the result set like aggregate functions do. They are essential for performing analytical queries, such as ranking, cumulative sums, and comparisons across rows, and provide flexibility for complex reporting and data analysis tasks.