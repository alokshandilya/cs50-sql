# Relating

## Notes link

- [Notes](https://cs50.harvard.edu/sql/2024/notes/1/)

## SQL Commands, Keywords, and Functions

- `IN` - used to check if a value is in a list of values, eg:
  - `SELECT * FROM "table" WHERE "column" IN (value1, value2, value3);`
  - difference from `=` is that `=` checks for exact match, while `IN` checks for any match
- `JOIN` - used to combine rows from two or more tables based on a related column between them, eg:
  - `SELECT * FROM "table1" JOIN "table2" ON "table1.column" = "table2.column";`
  - `JOIN` is a keyword, `ON` is a clause
  - `JOIN` is a type of inner `JOIN`, other types are `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, and `FULL JOIN`
  - difference b/w inner and outer `JOIN` is that inner `JOIN` only returns rows that have matching values in both tables, while outer `JOIN` returns all rows from both tables
  - `LEFT JOIN` returns all rows from the left table, and the matched rows from the right table, while `RIGHT JOIN` returns all rows from the right table, and the matched rows from the left table
  - `OUTER JOIN` returns all rows when there is a match in either table
  - `NATURAL JOIN` is a `JOIN` that performs a `JOIN` implicitly based on the columns that have the same name in both tables
- `UNION` - used to combine the result-set of two or more `SELECT` statements, eg:
  - `SELECT * FROM "table1" UNION SELECT * FROM "table2";`
  - `UNION` removes duplicate rows, to include duplicates use `UNION ALL`
- `INTERSECT` - used to combine the result-set of two or more `SELECT` statements, eg:
  - `SELECT * FROM "table1" INTERSECT SELECT * FROM "table2";`
  - `INTERSECT` returns only the rows that are present in both result-sets
- `EXCEPT` - used to combine the result-set of two `SELECT` statements, eg:
  - `SELECT * FROM "table1" EXCEPT SELECT * FROM "table2";`
  - `EXCEPT` returns only the rows that are present in the first result-set but not in the second
- `GROUP BY` - used to group rows that have the same values into summary rows, eg:
  - `SELECT "column1", "column2", COUNT("column3") FROM "table" GROUP BY "column1", "column2";`
  - `GROUP BY` is used with aggregate functions to group the result-set by one or more columns
- `HAVING` - used to filter the result-set groups that have the specified condition, eg:
  - `SELECT "column1", "column2", COUNT("column3") FROM "table" GROUP BY "column1", "column2" HAVING COUNT("column3") > 1;`
  - `HAVING` is used instead of `WHERE` with aggregate functions
    - `WHERE` is used before `GROUP BY`, `HAVING` is used after `GROUP BY`
    - `WHERE` clause filters individual rows before grouping
    - `HAVING` clause filters grouped data after aggregation
- **Execution Order**:
  - The clauses are applied in a specific sequence:
    - **WHERE** clause (filters individual rows)
    - **GROUP BY** clause (groups rows)
    - **HAVING** clause (filters grouped data)

```sql
-- WHERE clause (filters before grouping)
SELECT * FROM BOOKS WHERE PRICE > 350;

-- HAVING clause (filters after grouping)
SELECT COUNT(BOOK_ID), LANGUAGE 
FROM BOOKS 
GROUP BY LANGUAGE 
HAVING COUNT(LANGUAGE) > 1;
```

## Joins

### **1. Inner Joins**
An **inner join** returns only the rows that have matching values in both tables. Rows that do not have a match in either table are excluded from the result.

#### Types of Inner Joins:
- **`INNER JOIN`:**
  - Explicitly specifies an inner join.
  - Returns only rows where there is a match between the joined tables.
  
  ```sql
  SELECT *
  FROM TableA a
  INNER JOIN TableB b ON a.key = b.key;
  ```

- **`NATURAL JOIN`:**
  - Automatically performs an inner join based on columns with the same name in both tables.
  - Only includes rows where there is a match between the tables.
  
  ```sql
  SELECT *
  FROM TableA
  NATURAL JOIN TableB;
  ```

- **`CROSS JOIN`:**
  - Produces a Cartesian product of the two tables (all possible combinations of rows).
  - While technically not an "inner join," it is often grouped with inner joins because it doesn't involve filtering by matching rows.
  
  ```sql
  SELECT *
  FROM TableA
  CROSS JOIN TableB;
  ```

---

### **2. Outer Joins**
An **outer join** includes rows even if there is no match in one of the tables. It preserves unmatched rows from one or both tables, depending on the type of outer join.

#### Types of Outer Joins:
- **`LEFT OUTER JOIN` (or `LEFT JOIN`):**
  - Includes all rows from the left table (`TableA`) and matching rows from the right table (`TableB`).
  - If there is no match, the result will contain `NULL` for columns from the right table.
  
  ```sql
  SELECT *
  FROM TableA a
  LEFT JOIN TableB b ON a.key = b.key;
  ```

- **`RIGHT OUTER JOIN` (or `RIGHT JOIN`):**
  - Includes all rows from the right table (`TableB`) and matching rows from the left table (`TableA`).
  - If there is no match, the result will contain `NULL` for columns from the left table.
  
  ```sql
  SELECT *
  FROM TableA a
  RIGHT JOIN TableB b ON a.key = b.key;
  ```

- **`FULL OUTER JOIN` (or `FULL JOIN`):**
  - Includes all rows from both tables.
  - If there is no match, the result will contain `NULL` for columns from the table that lacks a match.
  
  ```sql
  SELECT *
  FROM TableA a
  FULL OUTER JOIN TableB b ON a.key = b.key;
  ```

---

### **Summary Table**

| Join Type          | Description                                                                                     | Includes Unmatched Rows? |
|--------------------|-------------------------------------------------------------------------------------------------|--------------------------|
| **`INNER JOIN`**   | Returns only rows with matching keys in both tables.                                           | No                       |
| **`NATURAL JOIN`** | Performs an inner join based on columns with the same name in both tables.                      | No                       |
| **`CROSS JOIN`**   | Produces a Cartesian product of the two tables.                                                | N/A                      |
| **`LEFT JOIN`**    | Includes all rows from the left table and matching rows from the right table.                   | Yes (from left table)    |
| **`RIGHT JOIN`**   | Includes all rows from the right table and matching rows from the left table.                   | Yes (from right table)   |
| **`FULL JOIN`**    | Includes all rows from both tables, filling in `NULL` where there is no match.                  | Yes (from both tables)   |

---

### **Key Points to Remember**
1. **Inner Joins** exclude unmatched rows.
   - Examples: `INNER JOIN`, `NATURAL JOIN`.

2. **Outer Joins** include unmatched rows.
   - Examples: `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`.

3. **`CROSS JOIN`** is neither inner nor outer—it produces all possible combinations of rows.

4. If you want to include rows with no match, use an **outer join** (`LEFT`, `RIGHT`, or `FULL`).

### Conclusion
- **Inner Joins:** `INNER JOIN (JOIN)`, `NATURAL JOIN`.
- **Outer Joins:** `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`.

## GROUP BY

The `GROUP BY` clause in SQL is used to group rows that have the same values in specified columns into summary rows. It is often used with aggregate functions like `COUNT()`, `SUM()`, `AVG()`, `MAX()`, and `MIN()` to perform calculations on each group of rows.

### **Key Points about `GROUP BY`:**
1. **Purpose:** It groups rows that share the same values in one or more columns.
2. **Aggregate Functions:** Used with aggregate functions to calculate summary statistics for each group.
3. **Common Use Cases:**
   - Counting occurrences.
   - Calculating totals, averages, or maximum/minimum values.
   - Summarizing data by categories.

---

### **Practical Example 1: Using `GROUP BY` Alone**

#### Scenario:
You have a table called `Orders` that contains information about customer orders. You want to find out how many orders each customer has placed.

#### Table: `Orders`
| order_id | customer_id | order_date | amount |
|----------|-------------|------------|--------|
| 1        | 101         | 2023-01-01 | 150    |
| 2        | 102         | 2023-01-02 | 200    |
| 3        | 101         | 2023-01-03 | 300    |
| 4        | 103         | 2023-01-04 | 100    |
| 5        | 102         | 2023-01-05 | 250    |

#### Query:
```sql
SELECT customer_id, COUNT(*) AS total_orders
FROM Orders
GROUP BY customer_id;
```

#### Explanation:
- The `GROUP BY customer_id` groups the rows by each unique `customer_id`.
- The `COUNT(*)` function counts the number of orders for each customer.

#### Result:
| customer_id | total_orders |
|-------------|--------------|
| 101         | 2            |
| 102         | 2            |
| 103         | 1            |

---

### **Practical Example 2: Using `GROUP BY` with Aggregate Functions**

#### Scenario:
You want to calculate the total amount spent by each customer.

#### Query:
```sql
SELECT customer_id, SUM(amount) AS total_spent
FROM Orders
GROUP BY customer_id;
```

#### Explanation:
- The `GROUP BY customer_id` groups the rows by each unique `customer_id`.
- The `SUM(amount)` function calculates the total amount spent by each customer.

#### Result:
| customer_id | total_spent |
|-------------|-------------|
| 101         | 450         |
| 102         | 450         |
| 103         | 100         |

---

### **Practical Example 3: Using `GROUP BY` with Joins**

#### Scenario:
You have two tables:
1. **`Customers`**: Contains customer details.
2. **`Orders`**: Contains order details.

You want to find out how much each customer has spent in total.

#### Tables:

**Customers**
| customer_id | name      |
|-------------|-----------|
| 101         | Alice     |
| 102         | Bob       |
| 103         | Carol     |

**Orders**
| order_id | customer_id | amount |
|----------|-------------|--------|
| 1        | 101         | 150    |
| 2        | 102         | 200    |
| 3        | 101         | 300    |
| 4        | 103         | 100    |
| 5        | 102         | 250    |

#### Query:
```sql
SELECT c.name, SUM(o.amount) AS total_spent
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.name;
```

#### Explanation:
- The `JOIN` combines the `Customers` and `Orders` tables based on the `customer_id`.
- The `GROUP BY c.name` groups the rows by each customer's name.
- The `SUM(o.amount)` calculates the total amount spent by each customer.

#### Result:
| name   | total_spent |
|--------|-------------|
| Alice  | 450         |
| Bob    | 450         |
| Carol  | 100         |

---

### **Practical Example 4: Using `GROUP BY` with Multiple Columns**

#### Scenario:
You want to find out how much each customer has spent in each year.

#### Query:
```sql
SELECT customer_id, YEAR(order_date) AS order_year, SUM(amount) AS total_spent
FROM Orders
GROUP BY customer_id, YEAR(order_date);
```

#### Explanation:
- The `GROUP BY customer_id, YEAR(order_date)` groups the rows by both `customer_id` and the year extracted from the `order_date`.
- The `SUM(amount)` calculates the total amount spent by each customer in each year.

#### Result:
| customer_id | order_year | total_spent |
|-------------|------------|-------------|
| 101         | 2023       | 450         |
| 102         | 2023       | 450         |
| 103         | 2023       | 100         |

---

### **Practical Example 5: Using `GROUP BY` with `HAVING`**

#### Scenario:
You want to find customers who have spent more than $200 in total.

#### Query:
```sql
SELECT customer_id, SUM(amount) AS total_spent
FROM Orders
GROUP BY customer_id
HAVING SUM(amount) > 200;
```

#### Explanation:
- The `GROUP BY customer_id` groups the rows by each unique `customer_id`.
- The `SUM(amount)` calculates the total amount spent by each customer.
- The `HAVING` clause filters the grouped results to include only customers whose total spending exceeds $200.

#### Result:
| customer_id | total_spent |
|-------------|-------------|
| 101         | 450         |
| 102         | 450         |

---

### **Practical Example 6: Using `GROUP BY` with Joins and `HAVING`**

#### Scenario:
You want to find customers who have placed more than one order.

#### Query:
```sql
SELECT c.name, COUNT(o.order_id) AS total_orders
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.name
HAVING COUNT(o.order_id) > 1;
```

#### Explanation:
- The `JOIN` combines the `Customers` and `Orders` tables based on the `customer_id`.
- The `GROUP BY c.name` groups the rows by each customer's name.
- The `COUNT(o.order_id)` counts the number of orders placed by each customer.
- The `HAVING` clause filters the grouped results to include only customers with more than one order.

#### Result:
| name   | total_orders |
|--------|--------------|
| Alice  | 2            |
| Bob    | 2            |

---

### **Key Takeaways:**
1. **`GROUP BY`** is used to group rows that share the same values in specified columns.
2. **Aggregate Functions** like `COUNT()`, `SUM()`, `AVG()`, etc., are often used with `GROUP BY` to calculate summary statistics for each group.
3. **Joins** can be combined with `GROUP BY` to summarize data across multiple tables.
4. The **`HAVING` clause** is used to filter grouped results, similar to how `WHERE` filters individual rows.

---

### **Final Answer:**
**`GROUP BY` is a powerful tool for summarizing data by grouping rows based on specific columns. It can be used alone or with joins, aggregate functions, and filtering conditions (`HAVING`) to produce meaningful insights from your data.**
