# Querying

## Notes Link

- [Notes](https://cs50.harvard.edu/sql/2024/notes/0/)

## SQL Commands

- `SELECT` - extracts data from a database
- `LIMIT` - limits the number of records returned
- `WHERE` - extracts only those records that fulfill a specified condition
  - `AND` - allows you to filter records based on more than one condition
  - `OR` - allows you to filter records based on more than one condition
  - `NOT` - allows you to filter records based on a condition that is not true
  - The operators that can be used to specify conditions in SQL are:
    - `=` - Equal
    - `>` - Greater than
    - `<` - Less than
    - `>=` - Greater than or equal
    - `<=` - Less than or equal
    - `<>` - Not equal
    - `!=` - Not equal
- `NULL` - represents missing or unknown data
  - conditions used with `NULL` are `IS NULL` and `IS NOT NULL`
    - `IS NULL` - returns true if the column is NULL
    - `IS NOT NULL` - returns true if the column is not NULL
- `LIKE` - a keyword used in SQL to search for a specified pattern in a column
  - `LIKE` - is used in a WHERE clause to search for a specified pattern in a column
  - `Wildcard Characters` - are used to search for a pattern within a column
    - `%` - represents zero, one, or multiple characters
    - `_` - represents a single character
- `ORDER BY` - sorts the result set in ascending or descending order
  - `ASC` - sorts the result set in ascending order, it is the default sort order
  - `DESC` - sorts the result set in descending order
- Aggregate Functions
  - `COUNT()` - returns the number of rows that matches a specified criteria
  - `SUM()` - returns the total sum of a numeric column
  - `AVG()` - returns the average value of a numeric column
  - `MIN()` - returns the smallest value of a numeric column
  - `MAX()` - returns the largest value of a numeric column
- `AS` - is used to rename a column or table

