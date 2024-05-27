# SQL Basics

## 1. Introduction to SQL

SQL (Structured Query Language) is a standard programming language specifically designed for managing and manipulating relational databases. It enables users to create, read, update, and delete data within a database. SQL is essential for data management in various applications, from simple web applications to complex enterprise systems.

### 1.1. Importance and Uses of SQL

- **Data Retrieval**: Extract data from databases for reporting and analysis.
- **Data Manipulation**: Insert, update, and delete data.
- **Data Definition**: Define database structures, such as tables and schemas.
- **Data Control**: Manage access to data through permissions and roles.

### 1.2. History and Evolution of SQL

SQL was initially developed at IBM in the early 1970s and has since become the standard language for relational database management systems (RDBMS). SQL standards are maintained by the American National Standards Institute (ANSI) and the International Organization for Standardization (ISO).

## 2. Fundamental SQL Commands

### 2.1. Data Manipulation Language (DML)

DML commands are used for managing data within schema objects. These commands allow you to perform tasks such as insert, update, delete, and retrieve data.

- **SELECT**: Retrieve data from a database.
- **INSERT**: Add new data into a table.
- **UPDATE**: Modify existing data within a table.
- **DELETE**: Remove existing data from a table.

#### 2.1.1. SELECT

The `SELECT` statement is used to retrieve data from one or more tables.
	
 ```
	SELECT column1, column2, ...
	FROM table_name;
```

#### 2.1.1. WHERE

The WHERE clause is used to filter records that meet specific conditions.

```
	SELECT column1, column2, ...
	FROM table_name
	WHERE condition;
```

#### 2.1.1. JOIN

SQL JOINs are used to combine rows from two or more tables based on a related column between them.

##### 2.1.1.1. INNER JOIN

Returns records that have matching values in both tables.

```
		SELECT a.column1, b.column2, ...
		FROM table1 a
		INNER JOIN table2 b
		ON a.common_field = b.common_field;
```

#### 2.1.1.2. LEFT JOIN

Returns all records from the left table and the matched records from the right table.

```
		SELECT a.column1, b.column2, ...
		FROM table1 a
		LEFT JOIN table2 b
		ON a.common_field = b.common_field;
```

#### 2.1.1.3. RIGHT JOIN

Returns all records from the right table and the matched records from the left table.

```
		SELECT a.column1, b.column2, ...
		FROM table1 a
		RIGHT JOIN table2 b
		ON a.common_field = b.common_field;
```

#### 2.1.1.4. FULL OUTER JOIN

Returns all records when there is a match in either left or right table.

```
		SELECT a.column1, b.column2, ...
		FROM table1 a
		FULL OUTER JOIN table2 b
		ON a.common_field = b.common_field;
```

#### 2.1.1.5. INSERT

The INSERT INTO statement is used to add new rows to a table.

```
		INSERT INTO table_name (column1, column2, ...)
		VALUES (value1, value2, ...);
```

#### 2.1.1.6. UPDATE

The UPDATE statement is used to modify existing records in a table.

```
		UPDATE table_name
		SET column1 = value1, column2 = value2, ...
		WHERE condition;
```

#### 2.1.1.7. DELETE

The DELETE statement is used to remove existing records from a table.

```
		DELETE FROM table_name
		WHERE condition;
```

### 2.2. Data Definition Language (DDL)

DDL commands are used to define and modify the structure of database objects. These commands allow you to create, alter, and drop schema objects such as tables, indexes, and views.

- CREATE: Create a new table, view, or other database object.
- ALTER: Modify an existing database object.
- DROP: Delete an existing database object.

#### 2.2.1. CREATE

Creates a new table or database.

```
		CREATE TABLE table_name (
			column1 datatype PRIMARY KEY,
			column2 datatype,
			...
		);
```

#### 2.2.2. ALTER

Modifies an existing database object, such as a table.

```
		ALTER TABLE table_name
		ADD column_name datatype;
```

#### 2.2.3. DROP

Deletes an existing database object, such as a table.

```
		DROP TABLE table_name;
```

### 2.3. Data Control Language (DCL)

DCL commands are used to control access to data within a database. These commands include granting and revoking permissions and managing user roles.

- GRANT: Provide specific privileges to users or roles.
		
```
		GRANT SELECT, INSERT ON dim_product TO user_name;
```
- REVOKE: Remove specific privileges from users or roles.
		
```
		REVOKE SELECT, INSERT ON dim_product FROM user_name;
```
