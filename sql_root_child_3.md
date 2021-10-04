---
title: Programming using PLSQL
---

<a href="./sql_root.html">SQL HOME</a>

# Table of Contents

1. [Advanced Aggregation Functions](#Section1)<br>
    1.1 [Ranking](#Section11)<br>
    1.2 [Windowing](#Section12)<br>
    1.3 [Pivoting](#Section13)<br>
    1.4 [Rollup](#Section14)<br>
2. [Control Flow Functions](#Section2)<br>
3. [Stored Procedures](#Section3)<br>
4. [Stored Functions](#Section4)<br>
5. [Triggers](#Section5)<br>

<a name=Section1></a>
# 1. Advanced Aggregation Functions

- The aggregation support in SQL is quite powerful and handles most common tasks with ease. 

- However, there are some tasks that are hard to implement efficiently with the basic aggregation features.

- Supports: Ranking, Windowing, Pivoting, Rollup

- Let's start by choosing our schema from the database. In our case it is dbsql.

    ```
    USE dbsql;
    ```

<a name=Section11></a>
## 1.1 Ranking:
- The ranking functions in MySql are used to rank each row of a partition.

- The ranking functions are also part of MySQL windows functions list.

- These functions are always used with OVER() clause.

- The ranking functions always assign rank on basis of ORDER BY clause.

- The rank is assigned to rows in a sequential manner.

- The assignment of rank to rows always start with 1 for every new partition.

- **Syntax:** SELECT col1, col2, ... [RANK(), DENSE_RANK(), PERCENT_RANK()] OVER (PARTITION BY col ORDER BY col DESC) FROM table_name;

    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS results (
        stu_name VARCHAR(20),
        subject_name VARCHAR(20),
        marks INT
    );
    ```
    ```
    -- Inserting fake data into the table
    INSERT INTO results VALUES ('Mukesh', 'Maths', 96);
    INSERT INTO results VALUES ('Ankita', 'Science', 80);
    INSERT INTO results VALUES ('Swarna', 'English', 100);
    INSERT INTO results VALUES ('Ankita', 'Maths', 96);
    INSERT INTO results VALUES ('Pratibha', 'Maths', 100);
    INSERT INTO results VALUES ('Hiren', 'English', 100);
    INSERT INTO results VALUES ('Ankita', 'Science', 80);
    INSERT INTO results VALUES ('Raj', 'English', 50);
    INSERT INTO results VALUES ('Mukesh', 'Science', 80);
    INSERT INTO results VALUES ('Raj', 'Science', 75);
    ```
    ```
    -- Example of RANK()
    SELECT subject_name, stu_name, marks, RANK() OVER (PARTITION BY subject_name ORDER BY marks DESC) AS 'rank' FROM results;
    ```
    ```
    -- Example of DENSE_RANK()
    SELECT subject_name, stu_name, marks, DENSE_RANK() OVER (PARTITION BY subject_name ORDER BY marks DESC) AS 'rank' FROM results;
    ```
    ```
    -- Example of PERCENT_RANK()
    SELECT subject_name, stu_name, marks, PERCENT_RANK() OVER (PARTITION BY subject_name ORDER BY marks DESC) AS 'rank' FROM results;
    ```

<a name=Section12></a>
## 1.2 Windowing:

- Window queries compute an aggregate function over ranges of tuples.

- This is useful, for example, to compute an aggregate of a fixed range of time; the time range is called a window.

- Windows may overlap, in which case a tuple may contribute to more than one window.

- **References:** https://www.w3schools.com/sql/sql_datatypes.asp

- Let's create a new table using some of the basic data types and insert some entries.

- **Syntax:** window_function_name (expression) OVER ([partition_defintion] [order_definition] [frame_definition])  
    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS sales (
        emp_name VARCHAR(45) NOT NULL,
        year INT NOT NULL,
        country VARCHAR(45) NOT NULL,
        product VARCHAR(45) NOT NULL,
        sale DECIMAL(12 , 2 ) NOT NULL,
        PRIMARY KEY (emp_name , year)
    );  
    ```
    ```
    -- Inserting fake data
    INSERT INTO sales VALUES ('Joseph', 2017, 'India', 'Laptop', 10000);
    INSERT INTO sales VALUES ('Joseph', 2018, 'India', 'Laptop', 15000);
    INSERT INTO sales VALUES ('Joseph', 2019, 'India', 'TV', 20000);
    INSERT INTO sales VALUES ('Bob', 2017, 'US', 'Computer', 15000);
    INSERT INTO sales VALUES ('Bob', 2018, 'US', 'Computer', 10000);
    INSERT INTO sales VALUES ('Bob', 2019, 'US', 'TV', 20000);
    INSERT INTO sales VALUES ('Peter', 2017, 'Canada', 'Mobile', 20000);
    INSERT INTO sales VALUES ('Peter', 2018, 'Canada', 'Calculator', 1500);
    INSERT INTO sales VALUES ('Peter', 2019, 'Canada', 'Mobile', 25000);
    ```
    ```
    SELECT SUM(sale) AS Total_Sales FROM sales;
    ```
    ```
    SELECT year, product, SUM(Sale) AS Total_Sales FROM sales GROUP BY Year ORDER BY product;  
    ```
    ```
    -- Similar to aggregate function, window function also works with a subset of rows, but it does not reduce the result set into a single row. 
    -- It means window functions perform operations on a set of rows and produces an aggregated value for each row.
    SELECT year, product, sale, SUM(sale) OVER (PARTITION BY year) AS Total_Sales FROM sales;
    ```

<a name=Section13></a>
## 1.3 Pivoting: 

- Pivot tables are useful for data analysis, allow you to display row values as columns to easily get insights. 

- However, there is no function to create a pivot table in MySQL. So, you need to write SQL query to create pivot table in MySQL. 

- Luckily there are many ways to create Pivot Table in MySQL.

    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS exams (
        id INT NOT NULL AUTO_INCREMENT,
        name VARCHAR(20),
        exam INT,
        score INT,
        PRIMARY KEY (id)
    );
    ```
    ```
    -- Inserting fake data into the exams table
    INSERT INTO exams (name, exam, score) VALUE ('Mukesh', 1, 70);
    INSERT INTO exams (name, exam, score) VALUE ('Mukesh', 2, 77);
    INSERT INTO exams (name, exam, score) VALUE ('Mukesh', 3, 71);
    INSERT INTO exams (name, exam, score) VALUE ('Mukesh', 4, 70);
    INSERT INTO exams (name, exam, score) VALUE ('Lakhan', 1, 89);
    INSERT INTO exams (name, exam, score) VALUE ('Lakhan', 2, 87);
    INSERT INTO exams (name, exam, score) VALUE ('Lakhan', 3, 88);
    INSERT INTO exams (name, exam, score) VALUE ('Lakhan', 4, 89);
    ```
    ```
    -- Creating Pivot Table using IF statement
    SELECT 
        name,
        SUM(IF(exam = 1, score, NULL)) AS exam1,
        SUM(IF(exam = 2, score, NULL)) AS exam2,
        SUM(IF(exam = 3, score, NULL)) AS exam3,
        SUM(IF(exam = 4, score, NULL)) AS exam4
    FROM
        exams
    GROUP BY name;
    ```
    ```
    -- Creating Pivot Table using CASE statement
    SELECT 
        name,
        SUM(CASE
            WHEN exam = 1 THEN score
            ELSE NULL
        END) AS exam1,
        SUM(CASE
            WHEN exam = 2 THEN score
            ELSE NULL
        END) AS exam2,
        SUM(CASE
            WHEN exam = 3 THEN score
            ELSE NULL
        END) AS exam3,
        SUM(CASE
            WHEN exam = 4 THEN score
            ELSE NULL
        END) AS exam4
    FROM
        exams
    GROUP BY name;
    ```

<a name=Section14></a>
## 1.4 Rollup:

- It is a modifier used to produce the summary output, including extra rows that represent super-aggregate (higher-level) summary operations.

- It enables us to sum-up the output at multiple levels of analysis using a single query.

- It is mainly used to provide support for OLAP (Online Analytical Processing) operations.

- The ROLLUP modifier can only be used with the GROUP BY query in the operation.

- **Syntax:** SELECT col1, col2, col3, ... FROM table_name GROUP BY col1, col2, col3, ... WITH ROLLUP;

    ```
    SELECT year, SUM(sale) AS Total_Sales FROM sales GROUP BY year;  
    ```
    ```
    -- In the above query, the grouping set is denoted by the column name year. 
    -- If we need to generate more than one grouping sets together in a single query, we may use the UNION ALL operator as follows:
    SELECT year, SUM(sale) AS Total_Sales FROM sales GROUP BY year 
    UNION ALL 
    SELECT NULL, SUM(sale) AS Total_Sales FROM sales; 
    ```
    ```
    -- The above query has two issues i.e. Lengthy query & performance decrease in the database engine.
    -- A better way to execute this would be using GROUP BY with ROLLUP operator
    SELECT year, SUM(sale) AS Total_Sales FROM sales GROUP BY year WITH ROLLUP;
    ```

<a name=Section2></a>
# 2. Control Flow Functions:

- The control flow function evaluates the condition specified in it. 

- The output generated by them can be a true, false, static value or column expression. 

- We can use the control flow functions in the SELECT, WHERE, ORDER BY, and GROUP BY clause.

- **Supports:** IF(), IFNULL(), NULLIF(), CASE

    ```
    -- Example of IF()
    SELECT IF(200 > 350, 'YES', 'NO') AS Result;
    ```
    ```
    -- Example of IFNULL()
    SELECT IFNULL(0, 5) AS Result;
    SELECT IFNULL('Hello', 'INSAID!') AS Result;
    SELECT IFNULL(NULL, 5) AS Result;
    ```
    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS student_contacts (
        sid INT NOT NULL AUTO_INCREMENT,
        contactname VARCHAR(45) NOT NULL,
        cellphone VARCHAR(20) DEFAULT NULL,
        homephone VARCHAR(20) DEFAULT NULL,
        PRIMARY KEY (sid)
    );
    ```
    ```
    -- Inserting fake data
    INSERT INTO student_contacts (contactname, cellphone, homephone) VALUES ('John Cena', '9876589542', '4569853652');
    INSERT INTO student_contacts (contactname, cellphone) VALUES ('Peter Thompson', '9176439542');
    INSERT INTO student_contacts (contactname) VALUES ('Kelly Bruke');
    INSERT INTO student_contacts (contactname, homephone) VALUES ('Freedo Pinto Cena', '4569853652');
    INSERT INTO student_contacts (contactname, cellphone, homephone) VALUES ('Mukesh Kumar', '9876589542', '4569853652');
    ```
    ```
    -- Example of IFNULL()
    SELECT contactname, IFNULL(cellphone, homephone) phone FROM student_contacts;
    ```
    ```
    -- Example of NULLIF(): Returns NULL if both expressions are same otherwise return first value
    SELECT NULLIF('INSAID', 'INSAID') AS Result;
    SELECT NULLIF('Hello', 'Mister!') AS Result;
    ```
    ```
    -- Example of CASE: 
    -- Syntax: CASE value WHEN [compare_value] THEN result [WHEN [compare_value] THEN result ...] [ELSE result] END  
    SELECT 
        CASE 1
            WHEN 1 THEN 'one'
            WHEN 2 THEN 'two'
            ELSE 'more'
        END AS Result;
    SELECT 
        CASE BINARY 'B'
            WHEN 'a' THEN 1
            WHEN 'b' THEN 2
        END AS Result;
    ```

<a name=Section3></a>
# 3. Stored Procedures:

- A collection of pre-compiled SQL statements stored inside the database.

- It is a subroutine or a subprogram in the regular computing language. 

- A procedure always contains a name, parameter lists, and SQL statements.

- An enterprise application always need to perform specific tasks such as database cleanup, processing payroll, and many more on the database regularly. 

- Such tasks involve multiple SQL statements for executing each task which could get easy if grouped together and stored in a database.

- **Syntax:** ```DELIMITER && 
		  CREATE PROCEDURE procedure_name [[IN | OUT | INOUT] parameter_name datatype [, parameter datatype])] 
          BEGIN
          Declaration_section 
          Executable_section 
          END && 
          DELIMITER ;```   

- Parameter Explanations:
    - **procedure_name:** It represents the name of the stored procedure.

    - **parameter:** It represents the number of parameters. It can be one or more than one.

    - **Declaration_section:** It represents the declarations of all variables.

    - **Executable_section:** It represents the code for the function execution.
    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS student_details (
        s_id INT NOT NULL AUTO_INCREMENT,
        s_code INT NOT NULL,
        s_name VARCHAR(20),
        s_subject VARCHAR(20),
        marks INT,
        phone BIGINT(11),
        PRIMARY KEY (s_id, s_code)
    );
    ```
    ```
    -- Inserting fake data
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (101, 'Mukesh', 'English', 96, 9658745698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (102, 'Mark', 'Physics', 70, 9658746298);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (103, 'Joseph', 'Maths', 70, 9658745698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (104, 'John', 'Maths', 85, 9642355698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (105, 'Barack', 'Maths', 80, 9623575698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (106, 'Rinky', 'English', 85, 9654562698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (107, 'Misty', 'Science', 92, 9625635698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (108, 'Brayan', 'Science', 75, 9658455698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (109, 'Ddario', 'Biology', 65, 9568745698);
    INSERT INTO student_details (s_code, s_name, s_subject, marks, phone) VALUES (110, 'Sam', 'Biology', 80, 9657856698);
    ```
    ```
    -- Procedure without Parameter
    -- Display records whose marks are greater than 80 and count all the table rows
    DELIMITER &&  
    CREATE PROCEDURE getMeritStudents ()  
    BEGIN  
        SELECT * FROM student_details WHERE marks > 80;  
        SELECT COUNT(s_code) AS Total_Student FROM student_details;    
    END &&  
    DELIMITER ;
    ```
    ```
    -- Calling the stored procedure
    CALL getMeritStudents();
    ```
    ```
    -- Procedures with OUT Parameter
    -- Get the maximum marks from the table using a MAX() function
    DELIMITER &&  
    CREATE PROCEDURE displayMaxMarks (OUT highestmarks INT)  
    BEGIN  
        SELECT MAX(marks) INTO highestmarks FROM student_details;   
    END &&  
    DELIMITER ;
    ```
    ```
    -- Calling the stored procedure
    CALL displayMaxMarks(@M);
    SELECT @M;
    ```
    ```
    -- Procedures with INOUT Parameter
    -- Get the marks from the table with the specified id and then stores it into the same variable var. 
    -- The var first acts as the IN parameter and then OUT parameter. Therefore, we can call it the INOUT parameter mode.
    DELIMITER &&  
    CREATE PROCEDURE displayMarks (INOUT var INT)  
    BEGIN  
        SELECT marks INTO var FROM student_details WHERE s_id = var;   
    END &&  
    DELIMITER ;
    ```
    ```
    -- Calling the stored procedure
    SET @M = '3';
    CALL displayMarks(@M);
    SELECT @M;
    ```
    ```
    -- list the stored procedures
    -- Syntax: SHOW PROCEDURE STATUS [LIKE 'pattern' | WHERE search_condition];
    SHOW PROCEDURE STATUS WHERE db = 'dbsql';
    ```
    ```
    -- Drop the stored procedure
    -- DROP PROCEDURE procedure_name;
    DROP PROCEDURE displayMarks;
    ```

<a name=Section4></a>
# 4. Stored Functions:

- They are the types of stored programs or set of SQL statements that perform some task/operation and return a single value.

- When you will create a stored function, you must have a CREATE ROUTINE database privilege. 

- **Syntax:** DELIMITER $$
		  CREATE FUNCTION fun_name(fun_parameter(s))  
		  RETURNS datatype  
		  [NOT] {Characteristics}  
		  fun_body;

- **Parameter Explanations:**

    - **fun_name:** It represents the name of the stored function in a database.

    - **fun_parameter:** It represents the number of parameters of function body. Parameter such as IN, OUT, INOUT are not allowed.

    - **datatype:** It is the data type of return value of the function.

    - **characteristics:** Supports (DETERMINISTIC, NO SQL, or READS SQL DATA) while using CREATE FUNCTION statement.

    - **fun_body:** A set of SQL statements to perform the operations. It requires at least one RETURN statement.

    ```
    -- Creating a new table people_
    CREATE TABLE IF NOT EXISTS people_ (
        id INT,
        p_name VARCHAR(20),
        age INT
    );
    ```
    ```
    -- Inserting fake data of people_
    INSERT INTO people_ VALUES (1, 'Mukesh', 19);
    INSERT INTO people_ VALUES (2, 'Rajesh', 18);
    INSERT INTO people_ VALUES (3, 'Lakhan', 16);
    INSERT INTO people_ VALUES (4, 'Jagdesh', 12);
    INSERT INTO people_ VALUES (5, 'Kamalpal', 20);
    ```
    ```
    -- A function to check if a particular person is adult (above 18) or not
    DELIMITER $$  
    CREATE FUNCTION checkAdult(
        age INT
    )
    RETURNS VARCHAR(20)
    DETERMINISTIC
    BEGIN
        DECLARE checkAdult VARCHAR(20);  
        IF (age < 18) THEN  
            SET checkAdult = 'Not Adult';   
        ELSEIF age < 30 THEN  
            SET checkAdult = 'Adult';  
        END IF;  
        -- return the customer occupation  
        RETURN (checkAdult);  
    END$$  
    DELIMITER ;
    ```
    ```
    -- Calling the stored function
    SELECT p_name, checkAdult(age) FROM people_ ORDER BY age;  
    ```

<a name=Section5></a>
# 5. Triggers:

- A set of SQL statements that reside in a system catalog.

- It is a special type of stored procedure that is invoked automatically in response to an event.

- Each trigger is associated with a table, which is activated on any DML statement such as INSERT, UPDATE, or DELETE. 

- A trigger is called a special procedure because it cannot be called directly like a stored procedure. 

- A trigger is called automatically when a data modification event is made against a table. 

- In contrast, a stored procedure must be called explicitly.

- In general, there are of two types triggers according to the SQL standard: Row-Level Trigger, Statement-Level Trigger.

    - **Row-Level Trigger:** It gets activated for each row by a triggering statement such as insert, update, or delete.

    - **Statement-Level Trigger:** It gets fired once for each event that occurs on a table regardless of how many rows are inserted, updated, or deleted.

- **Types of Triggers in MySQL:** There are maximum six types of actions or events in the form of triggers such as,

    - **Before Insert:** It is activated before the insertion of data into the table.

    - **After Insert:** It is activated after the insertion of data into the table.

    - **Before Update:** It is activated before the update of data in the table.

    - **After Update:** It is activated after the update of the data in the table.

    - **Before Delete:** It is activated before the data is removed from the table.

    - **After Delete:** It is activated after the deletion of data from the table.

- **Syntax:** CREATE TRIGGER trigger_name trigger_time trigger_event
              ON table_name FOR EACH ROW 
              BEGIN 
              --variable declarations 
              --trigger code 
              END;

- **Parameter Explanation:**

    - **trigger_name:** It represents the name of the trigger to create. 

    - **trigger_time:** The trigger action time, which should be either BEFORE or AFTER. 

    - **trigger_event:** The type of operation name that activates the trigger. It can be either INSERT, UPDATE, or DELETE operation. 

    - **table_name:** The name of table for which we wish to create a trigger. 

    - **BEGIN END Block:** The statement for execution when the trigger is activated. 

    ```
    -- Creating a new table
    CREATE TABLE IF NOT EXISTS employee (
        e_name VARCHAR(45) NOT NULL,
        occupation VARCHAR(35) NOT NULL,
        working_date DATE,
        working_hours VARCHAR(10)
    );
    ```
    ```
    -- Inserting fake data
    INSERT INTO employee VALUES ('Mukesh', 'Scientist', '2021-10-04', 9);  
    INSERT INTO employee VALUES ('Warner', 'Engineer', '2021-10-04', 10);
    INSERT INTO employee VALUES ('Peter', 'Actor', '2021-10-04', 9);
    INSERT INTO employee VALUES ('Marco', 'Doctor', '2021-10-04', 8);
    INSERT INTO employee VALUES ('Sankalp', 'Teacher', '2021-10-04', 10);
    INSERT INTO employee VALUES ('Anthany', 'Business', '2021-10-04', 11); 
    ```
    ```
    -- Trigger using BEFORE clause
    DELIMITER // 
    CREATE TRIGGER beforeInsertClause
    BEFORE INSERT ON employee FOR EACH ROW  
    BEGIN  
    IF NEW.working_hours < 0 THEN SET NEW.working_hours = 0;  
    END IF;  
    END // 
    ```
    ```
    -- Invoking TRIGGER with BEFORE clause
    INSERT INTO employee VALUES ('James', 'Farmer', '2021-10-08', 14); 
    INSERT INTO employee VALUES ('Romeo', 'Actor', '2021-10-12', -14); 
    ```
    ```
    -- Display the data of employee table
    SELECT * FROM employee;
    ```
    ```
    -- Creating two new tables student_info and student_detail
    CREATE TABLE IF NOT EXISTS student_info (
        stud_id INT NOT NULL,
        stud_code VARCHAR(15) DEFAULT NULL,
        stud_name VARCHAR(35) DEFAULT NULL,
        subject VARCHAR(25) DEFAULT NULL,
        marks INT DEFAULT NULL,
        phone VARCHAR(15) DEFAULT NULL,
        PRIMARY KEY (stud_id)
    );

    CREATE TABLE IF NOT EXISTS student_detail (
        stud_id INT NOT NULL,
        stud_code VARCHAR(15) DEFAULT NULL,
        stud_name VARCHAR(35) DEFAULT NULL,
        subject VARCHAR(25) DEFAULT NULL,
        marks INT DEFAULT NULL,
        phone VARCHAR(15) DEFAULT NULL,
        Lasinserted TIME,
        PRIMARY KEY (stud_id)
    ); 
    ```
    ```
    -- Trigger using AFTER clause
    DELIMITER //  
    CREATE TRIGGER afterInsertClause  
    AFTER INSERT ON student_info FOR EACH ROW  
    BEGIN  
    INSERT INTO student_detail VALUES (new.stud_id, new.stud_code, new.stud_name, new.subject, new.marks, new.phone, CURTIME());  
    END //
    ```
    ```
    -- Inserting a new entry which will be modified in the existing table
    INSERT INTO student_info VALUES (10, 110, 'Bob', 'Biology', 50, '8947346438'); 
    ```
    ```
    -- Display the table
    SELECT * FROM student_detail; 
    ```
