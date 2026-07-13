# SQL Cheat Sheet

Welcome to the SQL Cheat Sheet! This guide covers the foundational concepts and syntax of SQL (Structured Query Language).

## Overview

*   **SQL** stands for Structured Query Language. It is used to perform CRUD (Create, Read, Update, Delete) operations on databases.
*   **RDBMS** stands for Relational Database Management System. It is the basis for SQL and all modern database systems (e.g., MS SQL Server, IBM DB2, Oracle, MySQL, and Microsoft Access).

## Database Structure

A database most often contains one or more tables.

*   **Table:** A grid of cells arranged in rows and columns. *(Note: In the Ionic Catalog tab, we use the term **dataset**.)* SQL query results are returned in table format.
*   **Column:** A vertical stack of cells in a table. *(Note: In Ionic, we call these **fields**.)*
*   **Row:** A horizontal line of cells in a table. Each row represents a single record, or instance of the data being represented.
*   **Cell:** The intersection of a row and a column containing a single piece of data. 
    *   *Important note:* Cell data may have nested JSON, meaning more fields can be contained in one cell.

## Basic SQL Syntax Rules

*   SQL statements consist of keywords that are easy to understand.
*   SQL keywords are **NOT** case sensitive (e.g., `select` is the same as `SELECT`).
*   Always use a **semicolon (`;`)** after SQL statements to execute them.

## Most Important SQL Commands

*   `SELECT` - extracts data from a database
*   `UPDATE` - updates data in a database
*   `DELETE` - deletes data from a database
*   `INSERT INTO` - inserts new data into a database
*   `CREATE DATABASE` - creates a new database
*   `ALTER DATABASE` - modifies a database
*   `CREATE TABLE` - creates a new table
*   `ALTER TABLE` - modifies a table
*   `DROP TABLE` - deletes a table
*   `CREATE INDEX` - creates an index (search key)
*   `DROP INDEX` - deletes an index

---

## Querying Data

### SELECT Statement
```sql
SELECT column1, column2, ...
FROM table_name;
```
The `SELECT DISTINCT` statement is used to return only distinct (unique) values:
```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

### WHERE Clause
The `WHERE` clause is used to filter records. It extracts only those records that fulfill a specific condition.
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
*Text Fields vs. Numeric Fields Examples:*
```sql
SELECT * FROM Customers WHERE CustomerID = 1;
SELECT * FROM Customers WHERE CustomerID > 80;
```

### Operators
| Operator | Description | 
| :--- | :--- |
| `=` | Equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |
| `<>` | Not equal. *(Note: In some versions of SQL this operator may be written as `!=`)* |
| `BETWEEN` | Between a certain range |
| `LIKE` | Search for a pattern |
| `IN` | To specify multiple possible values for a column |

### ORDER BY
The `ORDER BY` keyword is used to sort the result-set in ascending (`ASC`) or descending (`DESC`) order. It sorts the result-set in ascending order by default.
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1, column2, ... ASC|DESC;
```
*Example:*
```sql
SELECT * FROM Customers
WHERE CustomerID = 1
ORDER BY Country ASC, CustomerName DESC;
```

---

## Logical Operators

### AND Operator
The `WHERE` clause can contain one or many `AND` operators. The `AND` operator is used to filter records based on more than one condition.
```sql
SELECT * FROM Customers
WHERE Country = 'Spain' AND CustomerName LIKE 'G%';
```

### OR Operator
The `WHERE` clause can contain one or more `OR` operators. The `OR` operator is used to filter records based on more than one condition.
```sql
SELECT * FROM Customers
WHERE Country = 'Germany' OR Country = 'Spain';
```

### NOT Operator
The `NOT` operator is used in the `WHERE` clause to return all records that DO NOT match the specified criteria. It reverses the result of a condition from true to false and vice-versa.
```sql
SELECT * FROM Customers
WHERE NOT Country = 'Spain';
```
*Note: The `NOT` operator is also used in combination with other operators to exclude data, such as: `NOT LIKE`, `NOT BETWEEN`, `NOT IN`, `IS NOT NULL`, and `NOT EXISTS`.*

---

## Handling NULL Values
If a field in a table is optional, it is possible to insert or update a record without adding any value to this field. This way, the field will be saved with a `NULL` value. 

A `NULL` value represents unknown, missing, or inapplicable data in a database field. It is not a value itself, but a placeholder to indicate the absence of data.

*IS NULL Syntax:*
```sql
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
```
*IS NOT NULL Syntax:*
```sql
SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL;
```

---

## Modifying Data

### UPDATE Statement
The `UPDATE` statement is used to update or modify one or more records in a table.
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### DELETE Statement
The `DELETE` statement is used to delete existing records in a table.
```sql
DELETE FROM table_name WHERE condition;
```



---

## SQL Aggregate Functions

An aggregate function performs a calculation on a set of values and returns a single summarized value. They are commonly paired with the `GROUP BY` clause to group results.

### Summary of Common Aggregate Functions
* `MIN()` - Returns the smallest value of a column.
* `MAX()` - Returns the largest value of a column.
* `COUNT()` - Returns the number of rows that match a criteria.
* `SUM()` - Returns the total sum of a numerical column.
* `AVG()` - Returns the average value of a numerical column.

---

### The `MIN()` Function
Works with numeric, string, and date data types.

```sql
SELECT MIN(column_name)
FROM table_name
WHERE condition;
```

*Example (Return lowest price):*
```sql
SELECT MIN(Price)
FROM Products;
```

---

### The `MAX()` Function
Works with numeric, string, and date data types.

```sql
SELECT MAX(column_name)
FROM table_name
WHERE condition;
```

*Example (Return highest price):*
```sql
SELECT MAX(Price)
FROM Products;
```

---

### The `COUNT()` Function
Counts records based on the argument passed to it:

```sql
SELECT COUNT([DISTINCT] column_name | *)
FROM table_name
WHERE condition;
```

#### Key COUNT Behaviors:
* `COUNT(*)` - Counts the total number of rows in the table (including rows with `NULL` values).
* `COUNT(column_name)` - Counts all non-null values inside the specified column.
* `COUNT(DISTINCT column_name)` - Counts only the unique, non-null values inside the column.

---

### The `SUM()` Function
Calculates the total sum of a numeric column, ignoring `NULL` values.

```sql
SELECT SUM(column_name)
FROM table_name
WHERE condition;
```

*Example (Summing product quantities):*
```sql
SELECT SUM(Quantity)
FROM OrderDetails;
```

---

### The `AVG()` Function
Calculates the mathematical average of a numeric column, ignoring `NULL` values.

```sql
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```

*Example (Calculating average price):*
```sql
SELECT AVG(Price)
FROM Products;
```

---

## Pattern Matching & Wildcards

### The `LIKE` Operator
The `LIKE` operator is used in a `WHERE` clause to search for a specified text pattern in a column. It relies on wildcard characters to describe the pattern.

```sql
-- Select all customers whose name starts with "a"
SELECT * FROM Customers
WHERE CustomerName LIKE 'a%';

-- Select all customers whose city contains "on"
SELECT * FROM Customers
WHERE City LIKE '%on%';
```

### SQL Wildcard Characters
Wildcard characters substitute one or more characters in a string:

| Symbol | Description | Example |
| :--- | :--- | :--- |
| `%` | Represents zero, one, or multiple characters. | `'a%'` (Starts with 'a') |
| `_` | Represents a single character. | `'h_t'` (Matches 'hot', 'hat', 'hit') |
| `[]` | Represents any single character within the brackets.* | `'[mst]%'` (Starts with 'm', 's', or 't') |
| `^` | Represents any character NOT inside the brackets.* | `'[^mst]%'` (Does not start with 'm', 's', or 't') |
| `-` | Represents any single character within the specified range.* | `'[a-c]%'` (Starts with 'a', 'b', or 'c') |
| `{}` | Represents any escaped character.** | Used to escape literal wildcard symbols. |

*\*Note: Support for brackets `[]`, negation `^`, and range `-` symbols may vary depending on the specific RDBMS (e.g., fully supported in SQL Server).*

---

## Advanced Selection: `IN`, `BETWEEN`, & Aliases

### The `IN` Operator
The `IN` operator allows you to specify multiple values in a `WHERE` clause. It acts as shorthand for multiple `OR` conditions.

```sql
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');
```

---

### The `BETWEEN` Operator
Filters values within a given range. The range is **inclusive** (meaning the beginning and end values are included). Values can be numbers, text, or dates.

```sql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;
```

#### Syntax:
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

---

### SQL Aliases (`AS`)
Aliases are used to give a table or a column a temporary, more readable name for the duration of a query.

```sql
-- Alias for columns
SELECT CustomerID AS ID, CustomerName AS Customer
FROM Customers;
```

#### Syntax:
* **Column Alias:**
  ```sql
  SELECT column_name AS alias_name
  FROM table_name;
  ```
* **Table Alias:**
  ```sql
  SELECT column_name(s)
  FROM table_name AS alias_name;
  ```
