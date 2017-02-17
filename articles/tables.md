# Tables
-	are the core objects of a relational database management system (RDBMS)
-	stores permanent data
-	have a solid structure consisting of columns
-	have data organized in records, which inherits the column-structure of the table
-	can have any number of records, can also be empty
-	must belong to a RDBMS schema
-	can be created (dropped, modified) with SQL-Commands – Data Definition Language (DDL)
-	data can be manipulated with SQL-Commands (SELECT, INSERT, UPDATE, DELETE, …) – Data Manipulation Language (DML)
-	best practice: consider the normalization rules when modeling tables!
-	best practice: try to avoid dependencies between the records of a table
-	best practice: create exact one table per entity of information. Example:
- all relevant information of a person ( birth_date, name, … ) should be places into one table persons and not in some diverse tables
- an table with information of persons and they orders should be divided into two tables persons and orders
-	best practice: tables with several hundreds of columns are terrible in handling – try to keep the number of columns as small as possible
-	best practice: use always column names in SQL-Statements! A single SELECT * FROM … can be a ticking bomb which explodes after adding a new column to the table persons.
-	best practice: it is easy to add a new column to a table (see above), but it can be a horror to drop a column (and keep the system running after that)



## Table creation
- Always create VARCHAR2 columns with CHAR: psn_first_name(varchar2 200 char)
