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


## Table creation
- Always create VARCHAR2 columns with CHAR: psn_first_name(varchar2 200 char)
