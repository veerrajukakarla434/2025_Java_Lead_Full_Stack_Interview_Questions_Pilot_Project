# SQL Basics

#### 1) What is SQL?

* Answer : Structured Query Language is a database tool which is used to create and access database to support software application.

# There are 3 types of SQL statements

#### DDL (Data Definition Language): 
* It is used to define the database structure such as tables. It includes three statements such as Create, Alter, and Drop.

#### Some of the DDL Commands are listed below

CREATE: It is used for creating the table.

CREATE TABLE table_name
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
ALTER: The ALTER table is used for modifying the existing table object in the database.

ALTER TABLE table_name
 ADD column_name datatype
OR

ALTER TABLE table_name
DROP COLUMN column_name

#### DML (Data Manipulation Language): 
* These statements are used to manipulate the data in records. Commonly used DML statements are **Insert, Update, and Delete.**

* The **Select statement** is used as partial DML statement that is used to select all or relevant records in the table.

#### DCL (Data Control Language): 
* These statements are used to set privileges such as Grant and Revoke database access permission to the specific user.

## What is a Data Definition Language?
* Data definition language (DDL) is the subset of the database which defines the data structure of the database in the initial stage when the database is about to be created. It consists of the following commands: CREATE, ALTER and DELETE database objects such as schema, tables, view, sequence, etc.

## What is a Data Manipulation Language?
*Data manipulation language makes the user able to retrieve and manipulate data. It is used to perform the following operations.

Insert data into database through INSERT command.
Retrieve data from the database through SELECT command.
Update data in the database through UPDATE command.
Delete data from the database through DELETE command.
## What is Data Control Language?
* Data control language allows you to control access to the database. DCL is the only subset of the database which decides that what part of the database should be accessed by which user at what point of time. It includes two commands GRANT and REVOKE.

GRANT: to grant the specific user to perform a particular task

REVOKE: to cancel previously denied or granted permissions.


## SQL Constraints

* **NOT NULL** - Ensures that a column cannot have a NULL value
* **UNIQUE** - Ensures that all values in a column are different
* **PRIMARY KEY** - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
* **FOREIGN KEY** - Uniquely identifies a row/record in another table
* **CHECK** - Ensures that all values in a column satisfies a specific condition
* **DEFAULT** - Sets a default value for a column when no value is specified
* **INDEX** - Used to create and retrieve data from the database very quickly

* Examples refer this link :
https://www.geeksforgeeks.org/sql-constraints/
https://www.studytonight.com/dbms/sql-constraints.php 
https://www.w3schools.com/sql/sql_constraints.asp
https://www.w3resource.com/sql/creating-and-maintaining-tables/creating-table-constraint.php

**Behaviour of Foriegn Key Column on Delete:**

* There are two ways to maintin the integrity of data in Child table, when a particular record is deleted in the main table. When two tables are connected with Foriegn key, and certain data in the main table is deleted, for which a record exits in the child table, then we must have some mechanism to save the integrity of data in the child table.

* **On Delete Cascade** : This will remove the record from child table, if that value of foriegn key is deleted from the main table.

* **On Delete Null** : This will set all the values in that record of child table as NULL, for which the value of foriegn key is deleted from the main table.

* If we don't use any of the above, then we cannot delete data from the main table for which data in child table exists. We will get an error if we try to do so.

**ERROR : Record in child table exist**

### DROP TABLE :
* The **DROP TABLE** command deletes a table in the database.

* The following SQL deletes the table "Shippers":
**DROP TABLE Shippers;**

### TRUNCATE TABLE:
* The **TRUNCATE TABLE** command deletes the data inside a table, but not the table itself.

* The following SQL truncates the table "Categories": 
**TRUNCATE TABLE Categories;**

### Delete Table:
* The DELETE Statement is used to delete rows from a table.
* **NOTE:** The WHERE clause in the sql delete command is optional and it identifies the rows in the column that gets deleted. If you do not include the WHERE clause all the rows in the table is deleted, so be careful while writing a DELETE query without WHERE clause.

# Difference between DELETE and TRUNCATE Statements:

* **DELETE Statement:** This command deletes only the rows from the table based on the condition given in the where clause or deletes all the rows from the table if no condition is specified. But it does not free the space containing the table.

* **TRUNCATE statement:** This command is used to delete all the rows from the table and free the space containing the table.

### When are we going to use truncate and delete?

* TRUNCATE is a DDL command, whereas DELETE is a DML command.
* We can’t execute a trigger in case of TRUNCATE whilst with DELETE, we can accomplish a trigger.
* TRUNCATE is quicker than DELETE, for the reason that when we use DELETE to delete the data, at that time it store the whole statistics in the rollback gap on or after where we can get the data back after removal. In case of TRUNCATE, it will not store data in rollback gap and will unswervingly rub it out. TRUNCATE do not recover the deleted data.
* **We can use any condition in WHERE clause using DELETE but it is not possible with TRUNCATE.5.If a table is referenced by any foreign key constraints, then TRUNCATE won’t work.**
