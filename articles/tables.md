# Tables
-	are the core objects of a relational database management system (RDBMS)
-	store permanent data
-	have a solid structure consisting of columns
-	have data organized in records, which inherit the column-structure of the table
-	can have any number of records or can be empty
-	must belong to a RDBMS schema
-	can be created (dropped, modified) with SQL-Commands – Data Definition Language (DDL)
-	data can be manipulated with SQL-Commands (SELECT, INSERT, UPDATE, DELETE, …) – Data Manipulation Language (DML)
-	best practice: ```consider the normalization rules when modeling tables!```
-	best practice: try to avoid dependencies between the records of a table
-	best practice: create exact one table per entity of information. Example:
  - all relevant information of a person ( birth_date, name, … ) should be placed into one table persons and not in many diverse tables
  - a table with information of persons and they orders should be divided into two tables persons and orders
-	best practice: tables with hundreds of columns are difficult in handling – try to keep the number of columns as small as possible
-	best practice: ```always use column names in SQL-Statements!``` A single SELECT * FROM … is a ticking bomb which explodes after adding a new column to the table persons.
-	best practice: it is easy to add a new column to a table (see above), but it can be a horror to drop a column (and keep the system running after that)

## The columns of a table can be grouped in following sections:

###	Primary Key (PK) Columns:
-	the value of the PK identifies a record – it must be unique for all records of the table
-	only one PK per table
-	can be built up of one or more columns (composite key)
- composite keys should have as few columns as possible - each additional column makes the key ineffective
-	the value of a PK should not depend from other data columns
-	once created the value of a PK should not be changed
- best practice: ```every table should have a PK!```
- best practice: PK columns are always NOT NULL
-	best practice: a surrogated PK, an extra single number column provided from a sequence with only one role – to build the PK, for example: id INT NOT NULL PRIMARY KEY 

###	Unique Key (UK) Columns:
-	UK is similar to the PK - the value of the UK identifies a record
-	Unlike the PK it is not problematic, if the value of the UK changes – the old and the new value must be unique for all records of the table
-	can be built up of one or more columns (composite key)
- composite keys should have as few columns as possible - each additional column makes the key ineffective
-	UK is optional
-	there can be one or more UKs per table
-	best practice: ```every table with a surrogate PK must have at least one UK!```
-	best practice: try to use as many UKs as possible – they increase the quality of data
-	best practice: for big tables keep in mind – every UK slows down the performance of  DML operations

###	Foreign Key (FK) Columns:
-	connects records of a table with records of other tables 
-	both tables in a FK relation can also be the same table (self-reference FK)
-	best practice: ```try to use as many FKs as possible – they increase the quality of data```
-	best practice: for big tables keep in mind – every FK slows down the performance of DML operations
-	best practice: be careful with self-reference FKs – DML operations, especially deletes, can get very complex
-	best practice: use the PK of the foreign table in foreign key definition

###	Data Columns
-	net data
-	builds the core of the information of every record
-	are used in UKs, sometimes also in PKs and FKs
-	the value of a data columns should not depend on values of other columns 
-	best practice: ```always use the accurate data type for every column! For example always create VARCHAR2 columns with CHAR: psn_first_name(varchar2 200 char)```

###	Other Columns
-	special columns, for example user_created to store the information which RDBMS-user has created this record
-	provided in the background
-	values are generated automatically instead of user input 
