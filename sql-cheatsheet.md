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

## Core SQL Clauses
The `SELECT` statement is used to extract data from a database.

```sql
SELECT column1, column2, ...
FROM table_name;
```
The `SELECT DISTINCT` statement is used to return only distinct (unique) values.

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

