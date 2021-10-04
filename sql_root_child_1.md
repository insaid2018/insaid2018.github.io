---
title: DB Schema & Querying in SQL
---

<a href="./sql_root.html">SQL HOME</a>

# Table of Contents

1. [SQL Data Types & Schema](#Section1)<br>
    1.1 [SQL Schema](#Section11)<br>
    1.2 [SQL Data Types](#Section12)<br>
2. [Types of SQL Commands](#Section2)<br>
    2.1 [Data Definition Language (DDL)](#Section21)<br>
    2.2 [Data Manipulation Language (DML)](#Section22)<br>
    2.3 [Data Control Language (DCL)](#Section23)<br>
    2.4 [Transaction Control Language (TCL)](#Section24)<br>
    2.5 [Data Query Language (DQL)](#Section25)<br>
3. [SQL Basic Operations](#Section3)<br>
    3.1 [The Rename Operation](#Section31)<br>
    3.2 [String Operations](#Section32)<br>
    3.3 [Tuples Ordering](#Section33)<br>
    3.4 [Where-Clause Predicates](#Section34)<br>
    3.5 [Set Operations](#Section35)<br>
4. [SQL Aggregate Functions](#Section4)<br>
 

<a name=Section1></a>
# 1. SQL Data Types & Schema

- In this part, we will see what are the basic data types that are widely used in SQL.

- We will see the creation of database schema.

<a name=Section11></a>
## 1.1 SQL Schema

- A schema a collection of database objects associated with a database.

- **Syntax:** ```CREATE SCHEMA schema_name;```

    ```
    CREATE SCHEMA dbsql;
    USE dbsql;
    ```
<a name=Section12></a>
## 1.2 SQL Data Types

- They defines what value the columns in a relation or table can hold.

- For example, it could be integer, character, date and time, binary, and so on.

- **References:** https://www.w3schools.com/sql/sql_datatypes.asp

- Let's create a new table using some of the basic data types and insert some entries.

- **Syntax (Table Creation):** ```CREATE TABLE table_name (col1 datatype, col2 datatype, ...);```

- **Syntax (Entries Insertion):** ```INSERT INTRO table_name VALUES (value1, value2, value3, ...);```

    ```
    -- Creating a new table with custom attributes having specific data types
    CREATE TABLE candidates (
        PID INT,
        first_name VARCHAR(20),
        last_name VARCHAR(20),
        address VARCHAR(50),
        city VARCHAR(20),
        salary INT
    );
    ```
    ```    
    -- Creating some fake data for candidates table 
    INSERT INTO candidates VALUES (101, 'Mukesh', 'Kumar', 'JCT Colony', 'Phagwara', 30000);
    INSERT INTO candidates VALUES (102, 'Lakhan', 'Singh', '152 Street', 'Paris', 110152);
    INSERT INTO candidates VALUES (103, 'Akash', 'Kumar', 'New Model Town', 'Phagwara', 50000);
    INSERT INTO candidates VALUES (104, 'Dheeraj', 'Bansal', 'Downtown', 'Dubai', 699778);
    INSERT INTO candidates VALUES (105, 'Raman', 'Singh', 'Yonge Street', 'Ontario', 210000);
    ```

- Let's view the data type of all the attributes that we just created.

- **Syntax:** ```SELECT DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'table_name';```

    ```
    -- To get a view of data types of our newly created table with fake data
    SELECT COLUMN_NAME, DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'candidates';
    ```

<a name=Section2></a>
# 2. Types of SQL Commands

- There are in total five types of SQL commands:

    - **Data Definition Language (DDL):** They are used to perform changes in the structure of the table.

    - **Data Manipulation Language (DML):** They are used to modify all form of changes in the database.

    - **Data Control Language (DCL):** They are used to grant and take back authority from any database user.

    - **Transaction Control Language (TCL):** They are used to manage transactions in the database.

    - **Data Query Language (DQL):** It is used to fetch the data from the database.

    - **References:** https://www.javatpoint.com/dbms-sql-command

<a name=Section21></a>
## 2.1 Data Definition Language (DDL)

- They are used to perform changes in the structure of the table.

- Supported Commands: CREATE, ALTER, DROP, TRUNCATE

- **CREATE:** It is used to create a new table in the database.
    - **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2, datatype, col3 datatype, ...);```

- **ALTER:** It is used to alter the structure of the database.
    - **Syntax:** ```ALTER TABLE table_name ADD/DROP/MODIFY (col1 datatype, col2 datatype, col3 datatype, ...);```

- **TRUNCATE:** It is used to delete all the rows from the table.
    - **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2, datatype, col3 datatype, ...);```

- **DROP:** It is used to delete both the structure and record stored in the table.
    - **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2, datatype, col3 datatype, ...);```

    ```
    -- Creating a new table employee 
    CREATE TABLE employees (
        eid INT,
        emp_name VARCHAR(20),
        emp_age INT,
        city VARCHAR(20),
        income VARCHAR(20)
    );
    ```
    ```
    -- Alternative way of creating a new table with a condition 
    CREATE TABLE IF NOT EXISTS employees (
        eid INT,
        emp_name VARCHAR(20),
        emp_age INT,
        city VARCHAR(20),
        income VARCHAR(20)
    );
    ```
    ```
    -- To add a new column of type INT in the table 
    ALTER TABLE employees ADD phone INT;
    ```
    ```
    -- To drop a column in the table 
    ALTER TABLE employees DROP phone;
    ```
    ```
    -- To modify the column of the table
    ALTER TABLE employees MODIFY income INT;
    ```
    ```
    -- To delete all the entries in the table
    TRUNCATE TABLE employees;
    ```
    ```
    -- To remove/delete/kill the table
    DROP TABLE employees;
    ```

<a name=Section22></a>
## 2.2 Data Manipulation Language (DML)

- They are used to modify all form of changes in the database.

- **Supported Commands:** INSERT, UPDATE, and DELETE

- **INSERT:** It is used to insert data into the row of a table.
    - **Syntax:** ```INSERT INTO table_name (col1, col2, col3, ...) VALUES (value1, value2, value3, ...);``` 

- **UPDATE:** It is used to update or modify the value of a column in the table.
    - **Syntax:** ```UPDATE table_name SET col1=val1, col2=val2, ... WHERE condition;```

- **DELETE:** It is used to remove one or more row from a table.
    - **Syntax:** ```DELETE FROM table_name;```

    ```
    -- Creating a new table employee 
    CREATE TABLE IF NOT EXISTS employees (
        eid INT,
        emp_name VARCHAR(20),
        emp_age INT,
        city VARCHAR(20),
        income INT
    );
    ```
    ```
    -- Inserting fake data into employees table 
    INSERT INTO employees (eid, emp_name, emp_age, city, income) VALUES (101, 'Peter', 32, 'New York', 20000);
    INSERT INTO employees (eid, emp_name, emp_age, city, income) VALUES (102, 'Mark', 32, 'California', 30000);
    INSERT INTO employees (eid, emp_name, emp_age, city, income) VALUES (103, 'Donald', 40, 'Arizona', 100000);
    INSERT INTO employees (eid, emp_name, emp_age, city, income) VALUES (104, 'Obama', 35, 'Florida', 500000);
    INSERT INTO employees (eid, emp_name, emp_age, city, income) VALUES (105, 'Linklon', 32, 'Georgia', 250000);
    ```
    ```
    -- Direct way of inserting fake data into employees table without specifying column names 
    INSERT INTO employees VALUES (101, 'Peter', 32, 'New York', 20000);
    INSERT INTO employees VALUES (102, 'Mark', 32, 'California', 30000);
    INSERT INTO employees VALUES (103, 'Donald', 40, 'Arizona', 100000);
    INSERT INTO employees VALUES (104, 'Obama', 35, 'Florida', 500000);
    INSERT INTO employees VALUES (105, 'Linklon', 32, 'Georgia', 250000);
    ```
    ```
    -- Update the entries in the employees table
    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    UPDATE employees SET emp_name='Raj', income=10000 WHERE eid=105;
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back
    ```
    ```
    -- Delete the entries in the employees table
    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    DELETE FROM employees;
    DELETE FROM employees WHERE eid=105;
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back
    ```

<a name=Section23></a>
## 2.3 Data Control Language (DCL)

- They are used to grant and take back authority from any database user.

- **Supported Commands:** GRANT, REVOKE

- **GRANT:** It is used to give user access privileges to a database.
    - **Syntax:** ```GRANT SELECT/INSERT/DELETE/INDEX/CREATE/ALTER/DROP/ALL/UPDATE ON level To user1, user2, ...;```

- **REVOKE:** It is used to take back permissions from the user.
    - **Syntax:** ```REVOKE SELECT/INSERT/DELETE/INDEX/CREATE/ALTER/DROP/ALL/UPDATE ON level FROM user1, user2, ...;```

    ```
    -- Creating a new user and assigning the privileges to the user
    CREATE USER IF NOT EXISTS 'mukesh'@'localhost' IDENTIFIED BY '123456789';
    ```
    ```
    -- Giving priviligies to the specified user
    GRANT SELECT, UPDATE ON * TO mukesh@localhost; -- Add Grant access at global level
    -- GRANT SELECT, UPDATE ON dbsql TO mukesh@localhost; -- Add Grant access at schema level
    -- GRANT SELECT, UPDATE ON dbsql.employees TO mukesh@localhost; -- Add Grant access at schema table level
    ```
    ```
    -- Show all the priviliges of specified user
    SHOW GRANTS FOR mukesh@localhost;
    ```
    ```
    -- Taking back the assigned privileges to the user
    REVOKE SELECT, UPDATE ON * FROM mukesh@localhost; -- Revoke Grant access at global level

    -- REVOKE SELECT, UPDATE ON dbsql FROM mukesh@localhost; -- Revoke Grant access at schema level
    -- REVOKE SELECT, UPDATE ON dbsql.employees FROM mukesh@localhost; -- Revoke Grant access at schema table level
    ```

<a name=Section24></a>
## 2.4 Transaction Control Language (TCL)

- They are used to manage database transactions and are only supported by commands DML commands.

- If the transaction makes multiple modifications into the database, two things happen:

    - Either all modification is successful when the transaction is committed.
    - Or, all modifications are undone when the transaction is rollback.

- **Supported Commands:** COMMIT, ROLLBACK, SAVEPOINT

- **COMMIT:** It is used to save all the transactions to the database.
    - **Syntax:** ```COMMIT;```
- **ROLLBACK:** It is used to undo transactions that have not already been saved to the database.
    - **Syntax:** ```ROLLBACK;```
- **SAVEPOINT:** It is used to roll the transaction back to a certain point without rolling back the entire transaction.
    - **Syntax:** ```SAVEPOINT savepoint_name;```

    ```
    -- Creating custom tables of employees and orders
    CREATE TABLE IF NOT EXISTS employees (
        eid INT,
        emp_name VARCHAR(20),
        emp_age INT,
        city VARCHAR(20),
        income INT
    );
    CREATE TABLE IF NOT EXISTS orders (
        oid INT,
        product_name VARCHAR(20),
        order_number INT,
        order_date DATE
    );
    ```
    ```
    -- Inserting fake data in employees table
    INSERT INTO employees VALUES (101, 'Peter', 32, 'New York', 20000);
    INSERT INTO employees VALUES (102, 'Mark', 32, 'California', 30000);
    INSERT INTO employees VALUES (103, 'Donald', 40, 'Arizona', 100000);
    INSERT INTO employees VALUES (104, 'Obama', 35, 'Florida', 500000);
    INSERT INTO employees VALUES (105, 'Linklon', 32, 'Georgia', 250000);
    INSERT INTO employees VALUES (106, 'Kane', 45, 'Alaska', 450000);
    INSERT INTO employees VALUES (107, 'Adam', 35, 'California', 500000);
    INSERT INTO employees VALUES (108, 'Macculam', 40, 'Florida', 100000);
    INSERT INTO employees VALUES (109, 'Brayan', 32, 'Alaska', 150000);
    INSERT INTO employees VALUES (110, 'Stephan', 40, 'Arizona', 160000);
    ```
    ```
    -- Inserting fake data in orders table
    INSERT INTO orders VALUES (1, 'Laptop', 5544, '2020-02-01');
    INSERT INTO orders VALUES (2, 'Mouse', 3322, '2020-02-11');
    INSERT INTO orders VALUES (3, 'Desktop', 2135, '2020-01-05');
    INSERT INTO orders VALUES (4, 'Mobile', 3432, '2020-02-22');
    INSERT INTO orders VALUES (5, 'Anivirus', 5648, '2020-03-10');
    ```
    ```
    -- Example of COMMIT in a transaction
    START TRANSACTION;

    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    UPDATE employees SET emp_name='Mukesh', emp_age=24 WHERE eid=101;
    UPDATE employees SET emp_name='Rajesh', emp_age=25 WHERE eid=103;
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back

    COMMIT;
    ```
    ```
    -- Example of ROLLBACK in a transaction
    START TRANSACTION;

    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    DELETE FROM orders;
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back

    SELECT * FROM orders;
    ROLLBACK;
    SELECT * FROM orders;
    ```
    ```
    -- Example of SAVEPOINT and ROLLBACK To in a transaction
    START TRANSACTION;

    SELECT * FROM orders;
    INSERT INTO orders VALUES (6, 'Printer', 5654, '2020-01-01');
    SAVEPOINT my_savepoint;
    SELECT * FROM orders;   

    INSERT INTO orders VALUES (7, 'Speaker', 5894, '2020-03-10');
    SELECT * FROM orders;
    ROLLBACK TO SAVEPOINT my_savepoint;
    SELECT * FROM orders;
    
    INSERT INTO orders VALUES (7, 'Headphones', 6065, '2020-03-10');
    SELECT * FROM orders;

    COMMIT;
    ```
<a name=Section25></a>
2.5 Data Query Language (DQL)

- It is used to fetch the data from the database.

- **Supported Commands:** SELECT

- **SELECT:** It is used to select the attribute based on the condition described by WHERE clause (optional).
    - **Syntax:** ```SELECT (col1, col2, col3, ...) FROM table_name [WHERE condition];```

    ```
    -- To view all the tuples in a table 
    SELECT * FROM orders;
    ```
    ```
    -- To view all the tuples with specific attributes in a table 
    SELECT product_name, order_date FROM orders;
    ```
    ```
    -- To view all the tuples with specific attributes along with a specific condidtion in a table 
    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    SELECT product_name, order_date FROM orders WHERE oid=6;
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back
    ```
<a name=Section3></a>
# 3. SQL Basic Operations

- There are a number of basic operations that are supported in SQL.

- **Supports:** Renaming, String operations, Tuples Ordering, Where-Clause Predicates, Set Operations, Null Values

<a name=Section31></a>
## 3.1 The Rename Operations

- To rename the resultant column in the result we use ```AS``` clause.

    ```   
    -- Example 1: The Rename Operation 
    SELECT emp_name AS Employee_Name, emp_age AS EMPLOYEE_AGE FROM employees;

    -- Example 2: The Rename Operation 
    SELECT E.emp_name, E.emp_age FROM employees AS E;

    -- Example 3: The Rename Operation 
    SELECT E.emp_name AS Employee_Name, E.emp_age AS EMPLOYEE_AGE FROM employees AS E;
    ```

<a name=Section32></a>
## 3.2 String Operations

- There are many string functions available in the MySQL that can be used to peform string manipulation.

- For reference you can use the official <a href="https://dev.mysql.com/doc/refman/8.0/en/string-functions.html">documentation</a> to get all the details about these functions.

    ```
    -- Example 1: String Operations (Using LOWER Function)
    SELECT LOWER(emp_name) AS lower_name FROM employees;

    -- Example 2: String Operations (Using UPPER Function)
    SELECT UPPER(emp_name) AS upper_name FROM employees;

    -- Example 3: String Operations (Using TRIM Function)
    SELECT TRIM(emp_name) AS trimmed_name FROM employees;
    ```
    ```
    -- String Operations (Pattern Matching with exact string)
    SELECT * FROM employees WHERE emp_name='Mukesh';
    ```
    ```
    -- String Operations (Pattern Matching with first letter) 
    SELECT * FROM employees WHERE emp_name LIKE 'M%';
    ```
    ```
    -- String Operations (Pattern Matching with last letter)
    SELECT * FROM employees WHERE emp_name LIKE '%h';
    ```
    ```
    -- String Operations (Pattern Matching with first and last letter only)
    SELECT * FROM employees WHERE emp_name LIKE 'M%h';
    ```
    ```
    -- String Operations (Pattern Matching with between characters only)
    SELECT * FROM employees WHERE emp_name LIKE '%es%';
    ```
    ```
    -- String Operations (Pattern Matching without first letter)
    SELECT * FROM employees WHERE emp_name LIKE '_ukesh';
    ```
    ```
    -- String Operations (Pattern Matching without last letter)
    SELECT * FROM employees WHERE emp_name LIKE 'Mukes_';
    ```
    ```
    -- String Operations (Pattern Matching without first and last letter)
    SELECT * FROM employees WHERE emp_name LIKE '_ukes_';
    ```
    ```
    -- String Operations (Pattern Matching using both % and _)
    SELECT * FROM employees WHERE emp_name LIKE '_u%';
    ```
    ```
    -- String Operations (Pattern Matching using both % and _)
    SELECT * FROM employees WHERE emp_name LIKE '%s_';
    ```
<a name=Section33></a>
## 3.3 Tuples Ordering

- Tuples ordering are one of the basic operations that are widely used in analyzing the data.

    ```
    -- Tuples Ordering with ORDER BY clause
    SELECT * FROM employees WHERE income < 50000 ORDER BY emp_name;
    ```
    ```
    -- Tuples Ordering with ORDER BY & DESC clause 
    SELECT * FROM employees WHERE income < 50000 ORDER BY emp_name DESC;
    ```
    ```
    -- Tuples Ordering with ORDER BY & ASC clause 
    SELECT * FROM employees WHERE income < 50000 ORDER BY income DESC, emp_name ASC;
    ```
    ```
    -- Where-Clause Predicates with BETWEEN & AND clause
    SELECT * FROM employees WHERE income BETWEEN 20000 AND 50000;
    ```
    ```
    -- Where-Clause Predicates with Arithmetic operator & AND clause
    SELECT * FROM employees WHERE income <= 20000 AND income >= 10000;
    ```

<a name=Section34></a>
## 3.4. Set Operations

- They are used to combine the two or more SQL SELECT statements.

- **Supports:** Union, UnionAll, Intersect, Minus

- **UNION:** It combine the result of two or more SQL SELECT queries and eliminates the duplicate rows from its resultset.
    - **Syntax:** ```SELECT col1, col2, ... FROM table1 UNION SELECT col1, col2, ... FROM table2;```

- **UNION ALL:** It combine the result of two or more SQL SELECT queries without eliminatings the duplicate rows from its resultset.
    - **Syntax:** ```SELECT col1, col2, ... FROM table1 UNION ALL SELECT col1, col2, ... FROM table2;```

- **INTERSECT (not supported in MySQL):** It combine the result of two or more SQL SELECT queries and returns the common rows from the SELECT statements.
    - **Syntax:** ```SELECT col1, col2, ... FROM table1 INTERSECT SELECT col1, col2, ... FROM table2;```

- **Minus (not supported in MySQL):** It combine the result of two or more SQL SELECT queries and display the rows which are present in the first query but absent in the second query.
    - **Syntax:** ```SELECT col1, col2, ... FROM table1 MINUS SELECT col1, col2, ... FROM table2;```

    ```
    -- Creating a two new tables and inserting some fake entries
    CREATE TABLE IF NOT EXISTS table1 (
        id INT,
        name VARCHAR(20)
    );

    CREATE TABLE IF NOT EXISTS table2 (
        id INT,
        name VARCHAR(20)
    );
    ```
    ```
    INSERT INTO table1 VALUES (1, 'Mukesh');
    INSERT INTO table1 VALUES (2, 'Rajesh');
    INSERT INTO table1 VALUES (3, 'Hiren');
    INSERT INTO table2 VALUES (1, 'Mukesh');
    INSERT INTO table2 VALUES (4, 'David');
    INSERT INTO table2 VALUES (5, 'Johnson');
    ```
    ```
    -- Example of UNION operation
    SELECT * FROM table1 UNION SELECT * FROM table2;
    ```
    ```
    -- Example of UNION ALL operation
    SELECT * FROM table1 UNION ALL SELECT * FROM table2;
    ```

<a name=Section4></a>
# 4. SQL Aggregate Functions

- They take a collection (a set or multiset) of values as input and return a single value.

- **Supported functions:** COUNT(), SUM(), AVG(), MIN(), MAX(), GROUP_CONCAT(), FIRST(), LAST()

- **COUNT():** It count the number of rows in a database table.

- **SUM():** It calculate the sum of all selected columns.

- **AVG():** It is used to calculate the average value of the numeric type.

- **MIN():** It is used to find the minimum value of a certain column.

- **MAX():** It is used to find the maximum value of a certain column.

- **GROUP_CONCAT():** It is used to concat two or more strings. It can be used along with some predicate.

- **FIRST() & LAST():** They show first and the last row in a database table. Not supported anymore. Instead LIMIT keyword is used.

    ```
    SELECT COUNT(eid) FROM employees;
    ```
    ```
    SELECT SUM(income) FROM employees;
    ```
    ```
    SELECT AVG(income) FROM employees;
    ```
    ```
    SELECT MIN(income) FROM employees;
    ```
    ```
    SELECT MAX(income) FROM employees;
    ```
    ```
    SELECT GROUP_CONCAT('First_Name', ' Last_Name') AS full_name;
    ```
    ```
    SELECT * FROM employees LIMIT 1;
    ```
    ```
    SELECT * FROM employees ORDER BY emp_name DESC LIMIT 1;
    ```
