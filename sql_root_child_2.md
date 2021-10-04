---
title: Advanced quering & db manipulations
---

<a href="./sql_root.html">SQL HOME</a>

# Table of Contents

1. [SubQueries](#Section1)<br>
    1.1 [Subqueries with the SELECT Statement](#Section11)<br>
    1.2 [Subqueries with the UPDATE Statement](#Section12)<br>
    1.3 [Subqueries with the DELETE Statement](#Section13)<br>
2. [Joins in SQL](#Section2)<br>
    2.1 [INNER JOIN](#Section21)<br>
    2.2 [LEFT OUTER JOIN](#Section22)<br>
    2.3 [RIGHT OUTER JOIN](#Section23)<br>
3. [Views in SQL](#Section3)<br>
4. [Keys in SQL](#Section4)<br>
    4.1 [Unique Key](#Section41)<br>
    4.2 [Primary Key](#Section42)<br>
    4.3 [Foreign Key](#Section43)<br>
    4.4 [Composite Key](#Section44)<br>

<a name=Section1></a>
# 1. SubQueries

- A subquery is a request for data or information from a database table or combination of tables in nested form.

- Subqueries are most frequently used with the SELECT statement.

- However you can use them within a INSERT, UPDATE, or DELETE statement as well, or inside another subquery.

- Few restrictions of nested queries:

    - A subquery must always appear within parentheses.

    - A subquery can have only one column in the SELECT clause, unless multiple columns are in the main query for the subquery to compare its selected columns.

    - An ORDER BY command cannot be used in a subquery, although the main query can use an ORDER BY. The GROUP BY command can be used to perform the same function as the ORDER BY in a subquery.

    - Subqueries that return more than one row can only be used with multiple value operators such as the IN operator.

    - The SELECT list cannot include any references to values that evaluate to a BLOB, ARRAY, CLOB, or NCLOB.

    - A subquery cannot be immediately enclosed in a set function.

    - The BETWEEN operator cannot be used with a subquery. However, the BETWEEN operator can be used within the subquery.

- Let's start by choosing our schema from the database. In our case it is dbsql

    ```
    USE dbsql;
    ```

    ```
    -- Creating a new table customers
    CREATE TABLE IF NOT EXISTS customers (
        id INT,
        c_name VARCHAR(20),
        age INT,
        location VARCHAR(20),
        salary INT
    );

    -- Creating a new table customers_bkup
    CREATE TABLE IF NOT EXISTS customers_bkup (
        id INT,
        c_name VARCHAR(20),
        age INT,
        location VARCHAR(20),
        salary INT
    );
    ```
    ```
    -- Inserting fake data into the table
    INSERT INTO customers VALUES (1, 'Mukesh', 24, 'Punjab', 30000);
    INSERT INTO customers VALUES (2, 'Ramesh', 35, 'Ahmedabad', 45000);
    INSERT INTO customers VALUES (3, 'Kaushik', 23, 'Kota', 25000);
    INSERT INTO customers VALUES (4, 'Hardik', 27, 'Mumbai', 50000);
    INSERT INTO customers VALUES (5, 'Komal', 24, 'Himachal', 20000);
    INSERT INTO customers VALUES (6, 'Hiren', 23, 'Mumbai', 30000);
    INSERT INTO customers VALUES (7, 'Lakhan', 25, 'Punjab', 100000);

    -- Inserting fake data into the table for backup table
    INSERT INTO customers_bkup VALUES (1, 'Mukesh', 24, 'Punjab', 30000);
    INSERT INTO customers_bkup VALUES (2, 'Ramesh', 35, 'Ahmedabad', 45000);
    INSERT INTO customers_bkup VALUES (3, 'Kaushik', 23, 'Kota', 25000);
    INSERT INTO customers_bkup VALUES (4, 'Hardik', 27, 'Mumbai', 50000);
    INSERT INTO customers_bkup VALUES (5, 'Komal', 24, 'Himachal', 20000);
    INSERT INTO customers_bkup VALUES (6, 'Hiren', 23, 'Mumbai', 30000);
    INSERT INTO customers_bkup VALUES (7, 'Lakhan', 25, 'Punjab', 100000);
    ```

<a name=Section11></a>
## 1.1 Subqueries with the SELECT Statement

- Subqueries are most frequently used with the SELECT statement.

- **Syntax:** ```SELECT col1, col2, ... FROM table1, table2, ... WHERE col OPERATOR (SELECT col1, col2, ... FROM table1, table2, WHERE condition);```

    ```
    SELECT * FROM customers WHERE id IN (SELECT id FROM customers WHERE salary > 20000);
    ```

<a name=Section12></a>
## 1.2 Subqueries with the UPDATE Statement

- The subquery can be used in conjunction with the UPDATE statement too.

- We can either use single columns or multiple columns in a table to update when using a subquery with the UPDATE statement.

- Here the nested table (target table) in the parentheses must be a different table than outside the query.

- **Syntax:** ```UPDATE table1 SET col1=val1, col2=val2, ... WHERE col OPERATOR (SELECT col1, col2, ... FROM table2 WHERE condition);```

    ```
    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    UPDATE customers SET salary = salary * 0.25 WHERE age IN (SELECT age FROM customers_bkup WHERE AGE >= 27);
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back
    ```

<a name=Section13></a>
## 1.3 Subqueries with the DELETE Statement

- The subquery can be used in conjunction with the DELETE statement like with any other statements mentioned above.

- Here the nested table (target table) in the parentheses must be a different table than outside the query.

- **Syntax:** ```DELETE FROM table WHERE col OPERATOR (SELECT col FROM table WHERE condition);```

    ```
    SET SQL_SAFE_UPDATES = 0; -- To disable safe_updates in SQL
    DELETE FROM customers WHERE age IN (SELECT age FROM customers_bkup WHERE age >= 27);
    SET SQL_SAFE_UPDATES = 1; -- To enable safe_updates in SQL back
    ```
<a name=Section2></a>
# 2. JOINS in SQL

- A JOIN clause is used to combine rows from two or more tables, based on a related column between them.

- There are three types of MySQL joins, Inner Join, Left Outer Join & Right Outer Join.

- Before we start let's create two new tables to understand joins.

    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS parents (
        pid INT,
        parent_name VARCHAR(20),
        location VARCHAR(20)
    );

    CREATE TABLE IF NOT EXISTS students (
        sid INT,
        student_name VARCHAR(20),
        course VARCHAR(20)
    );
    ```
    ```
    -- Inserting values in parents table
    INSERT INTO parents VALUES (1, 'Rakesh', 'Punjab');
    INSERT INTO parents VALUES (2, 'Deepika', 'Lucknow');
    INSERT INTO parents VALUES (3, 'Rahul', 'Delhi');
    INSERT INTO parents VALUES (4, 'Komal', 'Haryana');

    -- Inserting values in students table
    INSERT INTO students VALUES (1, 'Mukesh', 'Python');
    INSERT INTO students VALUES (2, 'Rohini', 'Java');
    INSERT INTO students VALUES (3, 'Rajesh', 'SQL');
    ```

<a name=Section21></a>
## 2.1 INNER JOIN

- It is used to return all rows from multiple tables where the join condition is satisfied.

- **Syntax:** ```SELECT col1, col2, col3, ... FROM table1 INNER JOIN table2 ON table1.column = table2.column;```

    ```
    SELECT parents.parent_name, parents.location, students.course FROM parents INNER JOIN students ON parents.pid = students.sid;
    ```

<a name=Section22></a>
## 2.2 LEFT OUTER JOIN

- It returns all rows from the left hand table specified in the ON the specified condition.

- Only those rows from the other table are returned where the join condition is satisfied.

- **Syntax:** ```SELECT col1, col2, col3, ... FROM table1 LEFT JOIN table2 ON table1.column = table2.column;```

    ```
    SELECT parents.parent_name, parents.location, students.course FROM parents LEFT JOIN students ON parents.pid = students.sid;
    ```

<a name=Section23></a>
## 2.3 RIGHT OUTER JOIN

- It returns all rows from the RIGHT-hand table specified in the ON specified condition.

- Only those rows from the other table are returned where the join condition is satisfied.

- **Syntax:** ```SELECT col1, col2, col3, ... FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;```

    ```
    SELECT parents.parent_name, parents.location, students.course FROM parents RIGHT JOIN students ON parents.pid = students.sid;
    ```

<a name=Section3></a>
# 3. VIEW in SQL

- The View is a virtual table created by a query by joining one or more tables.

- It is operated similarly to the base table but does not contain any data of its own.

- The View and table have one main difference that the views are definitions built on top of other tables (or views).

- If any changes occur in the underlying table, the same changes reflected in the View also.

    - **Syntax:** ```CREATE [REPLACE] VIEW view_name AS SELECT col1, col2, col3, ... FROM table [WHERE condition];```

    - **Syntax:** ```SELECT col1, col2, col3, ... FROM view_name [WHERE condition];```

    ```
    -- Creating a new view
    CREATE VIEW student_view AS SELECT * FROM students;
    ```
    ```
    -- Show the view
    SELECT * FROM student_view;
    ```
    ```
    -- UPDATE VIEW
    ALTER VIEW student_view AS SELECT * FROM students;
    ```
    ```
    -- DROP VIEW
    DROP VIEW IF EXISTS student_view;
    ```
    ```
    -- Create view with JOIN clause
    CREATE VIEW student_view AS SELECT parents.parent_name, parents.location, students.course 
    FROM parents LEFT JOIN students ON parents.pid = students.sid;
    ```
    ```
    -- Show the view
    SELECT * FROM student_view;
    ```

<a name=Section4></a>
# 4. Keys in SQL

- They are used to uniquely identify any record or row of data from the table. 

- It is also used to establish and identify relationships between tables.

- There are in total 4 types of keys that are available in MySQL, UNIQUE KEY, PRIMARY KEY, FOREIGN KEY, COMPOSITE KEY

<a name=Section41></a>
## 4.1 UNIQUE KEY

- It is a single field or combination of fields that ensure all values going to store into the column will be unique.
- **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2 datatype UNIQUE, col3 datatype, ...);```

    ```
    -- Example of UNIQUE Key
    CREATE TABLE IF NOT EXISTS people (id INT NOT NULL UNIQUE, p_name VARCHAR(20), age INT, city VARCHAR(20));
    ```

<a name=Section42></a>
## 4.2 PRIMARY KEY

- It is a single or combination of the field, which is used to identify each record in a table uniquely. 
- **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2 datatype PRIMARY KEY, col3 datatype, ...);```

    ```
    -- Example of PRIMARY Key
    CREATE TABLE IF NOT EXISTS login (login_id INT NOT NULL PRIMARY KEY, username VARCHAR(20), email VARCHAR(20), password VARCHAR(20));
    ```

<a name=Section43></a>
## 4.3 FOREIGN KEY

- It is used to link one or more than one table together. It is also known as the referencing key.
- **Syntax:** ```CREATE TABLE table_name (col1 datatype, col2 datatype, ... CONSTRAINT constraint_name 
                FOREIGN KEY (col) REFERENCES parent_table(col) ON [DELETE, UPDATE] referenceOption);```

    ```
    -- Example of FOREIGN Key
    CREATE TABLE IF NOT EXISTS customer (cid INT NOT NULL, c_name varchar(50) NOT NULL, city varchar(50) NOT NULL,  PRIMARY KEY (cid));  
    CREATE TABLE IF NOT EXISTS contact (
        id INT, cid INT, c_info VARCHAR(20), type VARCHAR(20), INDEX par_index (id), 
        CONSTRAINT fkey FOREIGN KEY (id) REFERENCES customer(cid) ON DELETE CASCADE ON UPDATE CASCADE 
    );
    ```

<a name=Section44></a>
## 4.4 COMPOSITE KEY

- It is a combination of two or more than two columns in a table that allows us to identify each row of the table uniquely.
- A composite key can be added in two ways: Using CREATE Statement or Using ALTER Statement
- **Syntax (Using CREATE):** ```CREATE TABLE table_name (col1 datatype, col2 datatype UNIQUE, ..., PRIMARY KEY (col1, col2, ...));```
- **Syntax (Using ALTER):** ```ALTER TABLE table_name ADD PRIMARY KEY (col1, col2, ...);``` 

    ```
    -- Example of COMPOSITE key (using CREATE)
    CREATE TABLE IF NOT EXISTS product (pid INT NOT NULL, p_name varchar(45), manufacturer varchar(45), PRIMARY KEY(p_name, manufacturer));
    ```
    ```
    -- Example of COMPOSITE key (using ALTER)
    CREATE TABLE IF NOT EXISTS student (sid INT NOT NULL, student_name varchar(45), subject varchar(45));
    ALTER TABLE student ADD PRIMARY KEY(sid, subject); 
    ```
