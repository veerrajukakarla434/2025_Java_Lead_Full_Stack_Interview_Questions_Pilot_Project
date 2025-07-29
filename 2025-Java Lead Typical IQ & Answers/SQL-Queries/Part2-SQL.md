# SQL Advanced:

**1) What is the Difference Between Unique and Distinct in SQL ?**

* **Unique** is a constraint in SQL that allows one or more fields or columns of a table to uniquely identify a record in a database table.
* **Distinct** is command used with ‘select’ that helps to return distinct or different values in the result set.

**Usage :**

* Furthermore, unique is a constraint that prevents two records from having identical values in a column, while distinct helps to remove duplicate values when retrieving data.

**Conclusion :**

* In brief, SQL is a language that allows performing operations on data stored in the database. Unique and Distinct are two SQL  constraints. The main difference between Unique and Distinct in SQL is that Unique helps to ensure that all the values in a column are different while Distinct helps to remove all the duplicate records when retrieving the records from a table. 

 **2) What is the Difference Between truncate and delete OR When are we going to use truncate and delete?**

* TRUNCATE is a DDL command, whereas DELETE is a DML command.
* We can’t execute a trigger in case of TRUNCATE whilst with DELETE, we can accomplish a trigger.
* TRUNCATE is quicker than DELETE, for the reason that when we use DELETE to delete the data, at that time it store the whole statistics in the rollback gap on or after where we can get the data back after removal. In case of TRUNCATE, it will not store data in rollback gap and will unswervingly rub it out. TRUNCATE do not recover the deleted data.
* **We can use any condition in WHERE clause using DELETE but it is not possible with TRUNCATE.5.If a table is referenced by any foreign key constraints, then TRUNCATE won’t work.**

**DELETE** | **TRUNCATE**
------------ | -------------
Delete command is used to delete a row in a table	| Truncate is used to delete all the rows from a table
You can rollback data after using delete statement	| You cannot rollback data
It is a DML command	| It is a DDL command

**3) What is an Index?**

* An index is performance tuning method of allowing faster retrieval of records from the table. An index creates an entry for each value and it will be faster to retrieve data.

* There are three types of indexes -.

**Unique Index:**
* This indexing does not allow the field to have duplicate values if the column is unique indexed. Unique index can be applied automatically when primary key is defined.

**Clustered Index:**
* This type of index reorders the physical order of the table and search based on the key values. Each table can have only one clustered index.

**NonClustered Index:**
* NonClustered Index does not alter the physical order of the table and maintains logical order of data. Each table can have 999 nonclustered indexes.

### SQL CREATE INDEX Statement
* The CREATE INDEX statement is used to create indexes in tables.
* Indexes are used to retrieve data from the database very fast. The users cannot see the indexes, they are just used to speed up searches/queries.

* **Note:** Updating a table with indexes takes more time than updating a table without (because the indexes also need an update). So, only create indexes on columns that will be frequently searched against.

* Creates an index on a table. Duplicate values are allowed.

```SQL
CREATE INDEX index_name ON table_name (column1, column2, ...);
```

* Creates a unique index on a table. Duplicate values are not allowed.

```SQL
CREATE UNIQUE INDEX index_name ON table_name (column1, column2, ...);
```

## SQL Joins

**SQL JOIN :**
* A JOIN clause is used to combine rows from two or more tables, based on a related column between them.

### Different Types of SQL JOINs 

* Here are the different types of the JOINs in SQL:

**1) (INNER) JOIN:** Returns records that have matching values in both tables

**2) LEFT (OUTER) JOIN:** Returns all records from the left table, and the matched records from the right table

**3) RIGHT (OUTER) JOIN:** Returns all records from the right table, and the matched records from the left table

**4) FULL (OUTER) JOIN:** Returns all records when there is a match in either left or right table


![img_innerjoin](https://www.w3schools.com/sql/img_innerjoin.gif "img_innerjoin")![img_leftjoin](https://www.w3schools.com/sql/img_leftjoin.gif "img_leftjoin")
![img_rightjoin](https://www.w3schools.com/sql/img_rightjoin.gif "img_rightjoin")
![img_fulljoin](https://www.w3schools.com/sql/img_fulljoin.gif "img_fulljoin")

**Consider the two tables below:**

* **Student Table**

![table1-3](https://media.geeksforgeeks.org/wp-content/cdn-uploads/table1-3.png "table1-3")

* **StudentCourse Table**

![table5](https://media.geeksforgeeks.org/wp-content/uploads/table5.png "table5")


* **The simplest Join is INNER JOIN.**

**1) INNER JOIN:** The INNER JOIN keyword selects all rows from both the tables as long as the condition satisfies. This keyword will create the result-set by combining all rows from both the tables where the condition satisfies i.e value of the common field will be same.

* **Syntax:**

```SQL
SELECT table1.column1,table1.column2,table2.column1,....
FROM table1 
INNER JOIN table2
ON table1.matching_column = table2.matching_column;
```
>table1: First table.
table2: Second table
matching_column: Column common to both the tables.

* Note: We can also write JOIN instead of INNER JOIN. JOIN is same as INNER JOIN.

![6a0120a85dcdae970b012877702708970c-pi](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png "6a0120a85dcdae970b012877702708970c-pi")

#### Example Queries(INNER JOIN)

* This query will show the names and age of students enrolled in different courses.

```SQL
SELECT StudentCourse.COURSE_ID, Student.NAME, Student.AGE FROM Student
INNER JOIN StudentCourse
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```

#### Output:

![table22](https://media.geeksforgeeks.org/wp-content/uploads/table22.png "table22")


**2) LEFT JOIN:** This join returns all the rows of the table on the left side of the join and matching rows for the table on the right side of join. The rows for which there is no matching row on right side, the result-set will contain null. LEFT JOIN is also known as LEFT OUTER JOIN.

* **Syntax:**

```SQL
SELECT table1.column1,table1.column2,table2.column1,....
FROM table1 
LEFT JOIN table2
ON table1.matching_column = table2.matching_column;
```

>table1: First table.
table2: Second table
matching_column: Column common to both the tables.

* Note: We can also use LEFT OUTER JOIN instead of LEFT JOIN, both are same.

![VkAT5](https://i.stack.imgur.com/VkAT5.png "VkAT5")

#### Example Queries(LEFT JOIN):

* We can also use LEFT OUTER JOIN instead of LEFT JOIN, both are same.

```SQL
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
LEFT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

**OR**

```SQL
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
LEFT JOIN StudentCourse 
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```

#### Output:

![table31](https://media.geeksforgeeks.org/wp-content/uploads/table31.png "table31")


**3) RIGHT JOIN:** RIGHT JOIN is similar to LEFT JOIN. This join returns all the rows of the table on the right side of the join and matching rows for the table on the left side of join. The rows for which there is no matching row on left side, the result-set will contain null. RIGHT JOIN is also known as RIGHT OUTER JOIN.

* **Syntax:**

```SQL
SELECT table1.column1,table1.column2,table2.column1,....
FROM table1 
RIGHT JOIN table2
ON table1.matching_column = table2.matching_column;
```

>table1: First table.
table2: Second table
matching_column: Column common to both the tables.

* Note: We can also use RIGHT OUTER JOIN instead of RIGHT JOIN, both are same.

![jk_JustSQL4_image004](http://www.databasejournal.com/img/jk_JustSQL4_image004.jpg "jk_JustSQL4_image004")

#### Example Queries(RIGHT JOIN):

```SQL
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
RIGHT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

#### Output:

![table6](https://media.geeksforgeeks.org/wp-content/uploads/table6.png "table6")

**4) FULL JOIN:** FULL JOIN creates the result-set by combining result of both LEFT JOIN and RIGHT JOIN. The result-set will contain all the rows from both the tables. The rows for which there is no matching, the result-set will contain NULL values.

* **Syntax:**

```SQL
SELECT table1.column1,table1.column2,table2.column1,....
FROM table1 
FULL JOIN table2
ON table1.matching_column = table2.matching_column;
```

>table1: First table.
table2: Second table
matching_column: Column common to both the tables.

![3Ll1h](https://i.stack.imgur.com/3Ll1h.png "3Ll1h")

#### Example Queries(FULL JOIN):

```SQL
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
FULL JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

#### Output:

![table7](https://media.geeksforgeeks.org/wp-content/uploads/table7.png "table7")


# SQL | Join (Cartesian Join & Self Join)

* In this article, we will discuss about the remaining two JOINS:

### CARTESIAN JOIN
### SELF JOIN

* Consider the two tables below:

![table2](https://media.geeksforgeeks.org/wp-content/cdn-uploads/table2.jpg "table2")


![table51](https://media.geeksforgeeks.org/wp-content/uploads/table51.png "table51")


**5) CARTESIAN JOIN:** The CARTESIAN JOIN is also known as CROSS JOIN. In a CARTESIAN JOIN there is a join for each row of one table to every row of another table. This usually happens when the matching column or WHERE condition is not specified.
In the absence of a WHERE condition the CARTESIAN JOIN will behave like a CARTESIAN PRODUCT . i.e., the number of rows in the result-set is the product of the number of rows of the two tables.

* In the presence of WHERE condition this JOIN will function like a INNER JOIN.
* Generally speaking, Cross join is similar to an inner join where the join-condition will always evaluate to True

* **Syntax:**
```SQL
SELECT table1.column1 , table1.column2, table2.column1...
FROM table1
CROSS JOIN table2;
```
##### Example Queries(CARTESIAN JOIN):

* In the below query we will select NAME and Age from Student table and COURSE_ID from StudentCourse table. In the output you can see that each row of the table Student is joined with every row of the table StudentCourse. The total rows in the result-set = 4 * 4 = 16.
```SQL
SELECT Student.NAME, Student.AGE, StudentCourse.COURSE_ID
FROM Student
CROSS JOIN StudentCourse;
```
#### Output:

![table_final](https://media.geeksforgeeks.org/wp-content/uploads/table_final.png "table_final")


**6) SELF JOIN:** As the name signifies, in SELF JOIN a table is joined to itself. That is, each row of the table is joined with itself and all other rows depending on some conditions. In other words we can say that it is a join between two copies of the same table.

* **Syntax:**
```SQL
SELECT a.coulmn1 , b.column2
FROM table_name a, table_name b
WHERE some_condition;

table_name: Name of the table.
some_condition: Condition for selecting the rows.
```

#### Example Queries(SELF JOIN):
```SQL
SELECT a.ROLL_NO , b.NAME
FROM Student a, Student b
WHERE a.ROLL_NO < b.ROLL_NO;
```
#### Output:

![tableeee1](https://media.geeksforgeeks.org/wp-content/uploads/tableeee1.png "tableeee1")

# SQL Queries On Salary

* **employee table**

**name** |**address**| **phone**|**salary**
------------ | ------------- | ------------- | -------------
mehar | hyd | 123456 | 96000
suresh | hyd | 987654321 | 85000
praveen | hyd | 777654321 | 86000
praveen | hyd | 777654321 | 88000
praveen | hyd | 777654321 | 88340
vamshi | hyd | 123456 | 68340
vamshi| hyd | 123456 | 48340
sheshu | hyd | 123456 | 78340

* **Query for Max salary**
```SQL
 select max(salary) from employee;
```
* **out put : 96000**

* **Query for Max salary with employee name**

```SQL
select name, salary from employee where salary = (select max(salary) from employee);
```

* **out put : mehar 96000**

* **Query for Min salary with employee name**

```SQL
select name, salary from employee where salary = (select min(salary) from employee);
```

* **out put : vamshi 48340**

* **Query for 2nd Max salary**

```SQL
select max(salary) from employee where salary not in (select max(salary) from employee);
```

 ###### OR

```SQL
SELECT MAX(Salary) From Employee WHERE Salary < ( SELECT Max(Salary) FROM Employee);
```

* **out put :  88340**


## Nth salary with other fields

```SQL
select name, salary from employee a where n-1 = (select count(salary)from employee b where a.salary<b.salary);
```

* Here you need to replace n value of the salary (2nd salary n=2, if 3rd highest salary n=3..etc)


```SQL
select name, salary from employee a where 2-1 = (select count(salary)from employee b where a.salary<b.salary);
```

**OR**

```SQL
SELECT name, Salary FROM Employee e WHERE 2=(SELECT COUNT(DISTINCT Salary) FROM Employee p WHERE e.Salary<=p.Salary);
```


##### Output: praveen 88340


**4) What are different Clauses used in SQL?**



![dbms-sql-clauses](https://static.javatpoint.com/dbms/images/dbms-sql-clauses.png "dbms-sql-clauses")

### Order of clauses while writing select queries:

![Select-Statement-Execution](https://i0.wp.com/www.complexsql.com/wp-content/uploads/2017/02/Select-Statement-Execution.png?w=548&ssl=1 "Select-Statement-Execution")


**1. The SQL WHERE Clause:**

* To limit the number of rows use the WHERE clause.
* The WHERE clause filters for rows that meet certain criteria.
* WHERE is followed by a condition that returns either true or false.
* WHERE is used with SELECT, UPDATE, and DELETE.

**Syntax :**

* A WHERE clause with a SELECT statement:

```SQL
SELECT column-names
  FROM table-name
 WHERE condition
 
 Example:
 
 SELECT Id, FirstName, LastName, City, Country, Phone
  FROM Customer
 WHERE Country = 'Sweden'
 
 ```
* A WHERE clause with an UPDATE statement:

```SQL
UPDATE table-name
   SET column-name = value
 WHERE condition
 
 Example :
 
 UPDATE Supplier
   SET City = 'Sydney'
 WHERE Name = 'Pavlova, Ltd.'
 
```
* A WHERE clause with a DELETE statement:

```SQL
DELETE table-name
 WHERE condition
 
 Example: 
 
 DELETE FROM Product
 WHERE UnitPrice > 50
 
```
**2. SQL ORDER BY Clause:** 

* SELECT returns records in no particular order.
* To ensure a specific order use the ORDER BY clause.
* ORDER BY allows sorting by one or more columns.
* Records can be returned in ascending or descending order.

**Syntax:**

```SQL
SELECT column-names
  FROM table-name
 WHERE condition
 ORDER BY column-names
 ```
 * Default order of "ORDER BY" is ASC

**3. SQL GROUP BY Clause**

* The GROUP BY statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

* The GROUP BY statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG) to group the result-set by one or more columns.

* The GROUP BY clause groups records into summary rows.
* GROUP BY returns one records for each group.
* GROUP BY typically also involves aggregates: COUNT, MAX, SUM, AVG, etc.
* GROUP BY can group by one or more columns.

**Syntax:**

```SQL
SELECT column-names
  FROM table-name
 WHERE condition
 GROUP BY column-names
 
(OR)With Order By
 
 SELECT column-names
  FROM table-name
 WHERE condition
 GROUP BY column-names
 ORDER BY column-names
 
```

* The following SQL statement lists the number of customers in each country:

Ref Link :https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby

```SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```
 
 Result :  https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby
 
* The following SQL statement lists the number of customers in each country, sorted high to low:

```SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```
Result : https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby_orderby

**GROUP BY With JOIN Example:**

#### OrderTable 

**OrderID** |**CustomerID**| **EmployeeID** |**OrderDate**|**ShipperID**				
------------ | ------------- | ------------- | ------------- | -------------
10248 |90 |5 |1996-07-04 |3
10249 |81 |6 |1996-07-05	|1
10250 |34 |4 |1996-07-08	|2

#### Shippers Table 

**ShipperID** |**ShipperName**			
------------ | ------------- 
1	| Speedy Express
2	| United Package
3	| Federal Shipping

* **The following SQL statement lists the number of orders sent by each shipper:**

```SQL
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;
```

Result link :https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_groupby1
 
Another Example:

**ORDER Table**

Id  OrderDate  OrderNumber  CustomerId TotalAmount

**CUSTOMER Table**

Id  FirstName  LastName  City  Country  Phone


* **Problem: List the total amount ordered for each customer**

```SQL
SELECT SUM(O.TotalPrice), C.FirstName, C.LastName
  FROM [Order] O JOIN Customer C 
    ON O.CustomerId = C.Id
 GROUP BY C.FirstName, C.LastName
 ORDER BY SUM(O.TotalPrice) DESC
 ```
 
**4.The SQL HAVING Clause**

* The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions. 
* HAVING filters records that work on summarized GROUP BY results.
* HAVING applies to summarized group records, whereas WHERE applies to individual records.
* Only the groups that meet the HAVING criteria will be returned.
* HAVING requires that a GROUP BY clause is present.
* WHERE and HAVING can be in the same query.
 
**Syntax:**

```SQL
 SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

* **The following SQL statement lists the number of customers in each country. Only include countries with more than 5 customers:**

```SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```
Result :https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_having

* **The following SQL statement lists the number of customers in each country, sorted high to low (Only include countries with more than 5 customers):**

```SQL
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5
ORDER BY COUNT(CustomerID) DESC;
```
Result : https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_having_orderby

* **Problem: List the number of customers in each country, except the USA, sorted high to low.
Only include countries with 9 or more customers.**

```SQL
SELECT COUNT(Id), Country 
  FROM Customer
 WHERE Country <> 'USA'
 GROUP BY Country
HAVING COUNT(Id) >= 9
 ORDER BY COUNT(Id) DESC
 ```
 
 Result: https://www.dofactory.com/sql/having
 
 ```SQL
 SELECT AVG(TotalAmount), FirstName, LastName
  FROM [Order] O JOIN Customer C ON O.CustomerId = C.Id
 GROUP BY FirstName, LastName
HAVING AVG(TotalAmount) BETWEEN 1000 AND 1200
```
Result: https://www.dofactory.com/sql/having

* **list out the employees name and whos salary is more than avg of employee's salary**

```SQL
select employeeId, employeeName 
from employee  
where salary <(select avg(salary)from employee)
group by employeeId, employeeName 
Order by employeeName;
```





