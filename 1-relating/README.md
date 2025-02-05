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
