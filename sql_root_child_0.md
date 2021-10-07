---
title: Introduction to SQL
---

<a href="./sql_root.html">SQL HOME</a>

# Introduction
 

SQL (Structured Query Language) is a domain-specific language used in programming and designed for managing data held in a relational database management system (RDBMS).
It is also used for stream processing in a relational data stream management system (RDSMS). 
It is particularly useful in handling structured data, i.e. data incorporating relations among entities and variables.

<center><img src="./images/sql-intro.jpeg"></center>

SQL offers two main advantages over older read–write APIs such as <a href="https://en.wikipedia.org/wiki/ISAM">ISAM</a> or <a href="https://en.wikipedia.org/wiki/Virtual_Storage_Access_Method">VSAM</a>. Firstly, it introduced the concept of accessing many records with one single command. 
Secondly, it eliminates the need to specify how to reach a record, e.g. with or without an index.

Originally based upon relational algebra and tuple relational calculus, SQL consists of many types of statements, which may be informally classed as sublanguages, commonly: 
- <a href="https://insaid2018.github.io/sql_root_child_1.html#Section25">Data query language (DQL)</a>, 
- <a href="https://insaid2018.github.io/sql_root_child_1.html#Section21">Data definition language (DDL)</a>, 
- <a href="https://insaid2018.github.io/sql_root_child_1.html#Section23">Data control language (DCL)</a>, 
- <a href="https://insaid2018.github.io/sql_root_child_1.html#Section22">Data manipulation language (DML)</a>

The scope of SQL includes data query, data manipulation (insert, update and delete), data definition (schema creation and modification), and data access control. 
Although SQL is essentially a declarative language (4GL), it also includes procedural elements.

SQL became a standard of the American National Standards Institute (ANSI) in 1986, and of the International Organization for Standardization (ISO) in 1987. 
Since then, the standard has been revised to include a larger set of features. 
Despite the existence of standards, most SQL code requires at least some changes before being ported to different database systems.
