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
