# SQL

Relational Database

## What is Database?
>Any collection of related information.

Computer + Databases = <3

**Database Management System (DBMS)**
- a special software program that helps users create and maintain a database.(create, read, update and delete)
  
C.R.U.D<br>
Create Read Update Delete

Two Types of Databases
1. Relational Database (SQL): Organize data into one or more tables
    - Each table has columns and rows
    - A unique key identifies each row
2. Non-Relational(noSQL / not just SQL): Organize data is anything but a traditional table
    - Key-value stores
    - Documents(JSON, XML, etc)
    - Graphs
    - Flexible Tables

Relational Databases(SQL)
- Relational Database Management Systems (RDBMS)
    - mySQL, Oracle, postgreSQL, mariaDB, etc.
- Structured Query Language (SQL)
    - Standardized language for interacting with RDBMS
    - Used to define tables and structures
  
Non- Relational Databases
- Document : JSON, BLOB, XML, etc
- Graph: Relational nodes
- Key-Value Hash: Keys are mapped to values(strings, json, blob, etc)
- Non-Relational Database Management System(NRDMBS)
    - mongoDB, dynamoDB, apache cassandra, firebase, etc

**Database Queries:** <br>
Queries are request made to the database management system for specific information.<br>
eg: a google search is a query

## Tables and Keys

|student_id|name|major|
|:------:|:----:|:---:|
|1|Jack|Biology|
|2|Kate|Sociology|
|3|Claire|English|

primary key: unique (studet_id)
forign key: connect to another database

## SQL Basics

>SQL is a language used for interacting with Relational Database Management System(RDBMS)

SQL is actually a hybrid language of 4 types:
1. Data Query Language
2. Data Definatin Language
3. Data Control Language
4. Data Manipulation Language

**Queries**<br>
A query is a set of instructions given to the RDBMS (written in SQL) that tell the RDBMS what information you wnat it to retrive for you.
```
SELECT employee.name, employee.age
FROM employee
WHERE employee.salary > 30000;
```

## Installation in mac

Google search : mysql community server (for download mysql)

```
echo 'export PATH=/usr/local/mysql/bin:$PATH' >> ~/.bash_profile
. ~/.bash_profile
mysql
mysql -u root -p
//Enter password : hint(QWone)

ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
// To update password
exit
```

To create database
```
create database databaseName;
```

Install PopSQL and use database name created abouve

## Creating Tables

**Data type in sql**<br>
INT             --Whole Numbers<br>
DECIMAL(M,N)    --Decimal Numbers - Exact Value<br>
VARCHAR(10)      --String of text of length 10<br>
BLOB            --Binary Large Object, Stores large data<br>
DATE            --'YYYY-MM-DD'<br>
TIMESTAMP       --'YYYY-MM-MM HH:MM:SS' - Used for recording <br>

Defining Database schema: 
```
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(30),
    major VARCHAR(20)
);
```
OR
```
CREATE TABLE student (
    student_id INT,
    name VARCHAR(30),
    major VARCHAR(20),
    PRIMARY KEY(student_id)
);
```

```
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(30),
    major VARCHAR(20)
);

DESCRIBE student;

DROP TABLE student;

ALTER TABLE student ADD gpa DECIMAL(3.2);

ALTER TABLE student DROP COLUMN gpa;
```

## Inserting Data

```
DROP TABLE student;

CREATE TABLE student (
    student_id INT,
    name VARCHAR(30),
    major VARCHAR(20),
    PRIMARY KEY(student_id)
);

SELECT * FROM student;

INSERT INTO student VALUES(1, 'jack', 'Biology');
INSERT INTO student VALUES(2, 'Kate', 'Sociology');
INSERT INTO student(student_id, name) VALUES(3, 'Claire');
INSERT INTO student VALUES(4, 'Jack', 'Biology');
INSERT INTO student VALUES(5, 'Mike', 'Computer Science');
```
NOT NULL / UNIQUE / DEFAULT / AUTO_INCREMENT
```
CREATE TABLE student (
    student_id INT AUTO_INCREMENT,
    name VARCHAR(30) NOT NULL,
    major VARCHAR(20) UNIQUE DEFAULT 'undecided',
    PRIMARY KEY(student_id)
);

INSERT INTO student(name) VALUES('jack');
```

## Update & Delete

```
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';

UPDATE student
SET major = 'Biochemistry'
WHERE major = 'Biology' OR major = 'Chemistrey';

UPDATE student
SET name = 'Tom', major = 'undecided'
WHERE student_id = 1;

DELETE from student
WHERE student_id = 5;
```

## Basic 
```
SELECT * FROM student;

SELECT name FROM student;

SELECT student.name, student.major
FROM student
ORDER BY name DESC; #ASC

SELECT * 
FROM student
WHERE student_id < 3;
```
Operations: <br>
--, <, >, <=, >=, =, <>, AND, OR

## Company Database Intro

## Creating Company Database

```
CREATE DATABASE tenzin;

CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);

DESCRIBE employee;

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

DESCRIBE branch;

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);

-- -----------------------------------------------------------------------------

-- Corporate
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);

INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);

SELECT * FROM employee;

-- Scranton
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

-- Stamford

INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);

INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');

UPDATE employee
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);

SELECT * FROM employee;
SELECT * FROM branch;

-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

-- CLIENT
INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
INSERT INTO client VALUES(401, 'Lackawana Country', 2);
INSERT INTO client VALUES(402, 'FedEx', 3);
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
INSERT INTO client VALUES(405, 'Times Newspaper', 3);
INSERT INTO client VALUES(406, 'FedEx', 2);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 55000);
INSERT INTO works_with VALUES(102, 401, 267000);
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);

-- -------------------------------------------------
-- Just to check 

SELECT * FROM employee;
SELECT * FROM branch;
SELECT * FROM client;
SELECT * FROM works_with;
SELECT * FROM branch_supplier;


```

## More Basic Queries
```
-- Find all employees
SELECT *
FROM employee;

-- Find all clients
SELECT *
FROM client;

-- Find all emplyoyees oredered by salary
SELECT *
FROM employee
ORDER BY salary DESC;

-- Find all employees ordered by sex then name
SELECT *
FROM employee
ORDER BY sex, first_name, last_name;

-- Find the first 5 employees in the table
SELECT * 
FROM employee
LIMIT 5;

-- Find the first and last names of all employees
SELECT first_name, last_name
FROM employee;

-- Find the first and last names of all employees as forename and surname
SELECT first_name AS forename, last_name AS surname
FROM employee;

-- Find out all the different genders
SELECT DISTINCT sex
FROM employee;
```

## Functions
```
-- Find the number of employees
SELECT COUNT(emp_id)
FROM employee;

-- Hoe many emplloyees has supervisor
SELECT COUNT(super_id)
FROM employee;

-- Find the number of female employees born after 1970
SELECT COUNT(emp_id)
FROM employee
WHERE sex = 'F' AND birth_day > '1971-01-01';

-- Find the sum of all employee's salaries
SELECT SUM(salary)
FROM employee;

-- Find the average of all employee's salaries
SELECT AVG(salary)
FROM employee;

-- Find the average of all employee who are male
SELECT AVG(salary)
FROM employee
WHERE sex = 'M';

-- Find out how many males and females there are
SELECT COUNT(sex), sex
FROM employee
GROUP BY sex;

-- Find the total sales of each salesman
SELECT emp_id, SUM(total_sales)
FROM works_with
GROUP BY emp_id;

--  Find the total spend of each client
SELECT client_id, SUM(total_sales)
FROM works_with
GROUP BY client_id;
```

## Wildcards
```
-- % = any number of characters, _ = one character

-- Find any client's who are in LLC
SELECT * 
FROM client
WHERE client_name LIKE '%Ex';

-- Find any branch suppliers who are in the label business
SELECT *
FROM branch_supplier
WHERE supplier_name LIKE '%lab%';

-- Find any employee born in February
SELECT *
FROM employee
WHERE birth_day LIKE '____-02%';

-- Find any clients who are schools
SELECT * 
FROM client
WHERE client_name LIKE '%school%';
```

## Union
```
-- Find list of employee and branch names
-- They have to same # of columns and shoud be similar datatype
SELECT first_name AS employee and branch name
FROM employee
UNION
SELECT branch_name
FROM branch;

-- Find the list of all client & branch suppliers name
SELECT client_name, branch_id
FROM client
UNION
SELECT supplier_name, branch_id
FROM branch_supplier;

-- Find a sum of all money spent or earned by the company
SELECT SUM(salary) AS salaryAndSales
FROM employee
UNION
SELECT SUM(total_sales)
FROM works_with;
```

## Joins
```
-- use to join rows on bases with related column

INSERT INTO branch VALUES(4, 'Buffalo', NULL, NULL);

SELECT * FROM branch;

-- Find all the branches and the names of their managers
SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id;

SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM employee
LEFT JOIN branch
ON employee.emp_id = branch.mgr_id;

SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM employee
RIGHT JOIN branch
ON employee.emp_id = branch.mgr_id;
```

## Nested Queries
```
-- Find names of all employees who have sold over 30,000 to a single client
-- Multiple select statements
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN (
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales  > 30000
);

-- Find all clients who are handled by the branch that Michael Scott manages 
-- Assume you know Micheel's ID = 102
SELECT client.client_name
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = 102
    LIMIT 1
);
```

## On Delete
```
-- on delete set null  -- will set to NULL
-- on delete cascade  -- will delete the rest

DELETE FROM employee
WHERE emp_id = 102; 
```

## Triggers
```
-- TRIGGERS

CREATE TABLE trigger_test (
    message VARCHAR(100)
);

DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee
    FOR EACH ROW BEGIN
        INSERT INTO trigger_test VALUES('added new employee');
    END$$
DELIMITER ;

INSERT INTO employee
VALUES(109, 'Oscar', 'Martinez', '1968-02-19', 'M', 6900, 106, 3);

SELECT * FROM trigger_test;
```
[link to more code and the video](https://www.mikedane.com/databases/sql/triggers/)

## ER Diagrams

ER Diagrams Intro (Entity Relation diagram)<br>
Relation model

**Entity-** An object we want to model & store information about. eg: Student
**Attributes-** Specific pieces of informatin about an entity. eg: name, grade, gpa

**Primary Key-** An attributes that uniquiley identify an entry in the database table. eg: <u>student_id</u>

**Composite Attribute-** An attribute that can be broken up into sub-attributes eg: name-> fname and lname

**Multi-valued Attribute-** An attribute that can have more than one value. eg: clubs (a student can have multiple clubs) symbol by double cirular enclosure

**Derived Attribute-** An attribute that can be derived from the other attributes. eg: has_honors

**Multiple Entities-** You can define more than one entity in the diagram. eg: Class

**Relationships-** defines a relationship between two entities. Student takes Class

**Total Participation-** All members must participate in the relationship

**Relationship Attribute-** An attribute about the relationship. grade when Studen takes Class

**Relationship Cardinality-** The number of instances of an entity from a reltion tht can be associated with the relation. 
- 1 : 1
- 1 : N
- N : M

**Weak Entity-** An entity that cannot be uniquily indetified by its attributes alone. Eg: Exam has Class

**Identifying relationship-** A relationship that serves to uniquly identify the weak entity.

[image](link)

## Designing an ER Diagram 
- Need adding
## Converting ER Diagram to Schemas
- Need adding






