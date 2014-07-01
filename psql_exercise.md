# SQL Commands Exercise

## Objectives
* Gain practice using Postgres SQL commands
* Understand CRUD
* Become familiar with Postgres documentation [Postgres 9.3 Docs](http://www.postgresql.org/docs/9.3/interactive/index.html)	

## CRUD (Create, Read, Update, Destroy)
  - __CRUD => Create, Read, Update, Destroy__ This is the lifecycle of data in an applicatoin.  In SQL, CRUD can be mapped to the following __INSERT, SELECT, UPDATE, DELETE__. 

```
  Create	INSERT
  Read		SELECT
  Update	UPDATE
  Destroy	DELETE
```

## Creating a Database

Most database products have the notion of separate databases.  Lets create one for the lesson.  In your terminal, type ```psql```.  Next create a database:

```
CREATE DATABASE testdb
```

Next, list all of the available databases:

```
\list
```

Now connect to the database we just created.

```
\connect testdb
```
Once we connect, our command prompt should look similar to this: ```testdb=#```

Check what tables we have in our newly created database:

```
\dt
```

At this point we should have a database with no tables in it.  So now we need to create tables.

## Creating a Table

Let's look at the Postgres docs for __[CREATE TABLE doc](http://www.postgresql.org/docs/9.3/static/sql-createtable.html).__

The basic structure for table creation:  

```
CREATE TABLE table_name
(
   column_name1 data_type(size),
   column_name2 data_type(size),
   column_name3 data_type(size),
   ....
);
```

Example:

This is an example of a students table.  We will talk about the primary key soon.

```
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone_no TEXT,
    email TEXT
);
```
#### What is a Primary Key?

It denotes an attribute on a table that can uniquely identify the row.

#### What does SERIAL Do?

SERIAL tells the database to automatically assign the next unused integer value to id whenever we insert into the database and do not specify id.  In general, if you have a column that is set to SERIAL, it is a good idea to let the database assign the  value for you.

#### Data Types

Similar to how Ruby has types of data, SQL defines types that can be stored in the DB. Here are some common ones:

* Serial
* Integer
* Numeric // Numbers are exact, no rounding error
* Float // Rounding error is possible, but operations are faster than Numeric
* Text, Varchar
* Timestamp
* Boolean (True or False)

### Dropping a Table

Let's say we no longer need the students table from above, to get rid of all of the data and the defiintion of the table, we can use the DROP statement.  Here are the [DROP TABLE doc](http://www.postgresql.org/docs/9.3/static/sql-droptable.html).

```
DROP TABLE students;
```

## Create Table (again)

```
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone_no TEXT,
    email TEXT
);
```


## INSERT (Create part of CRUD)

The INSERT SQL command adds new data to a table.  Here are the [INSERT doc](http://www.postgresql.org/docs/9.3/static/sql-insert.html).

Below is an example INSERT command.  It inserts a student into the students database.

```
INSERT INTO student (name, phone_no, email)
VALUES ("Spencer Eldred", "123-4567", "spencer@galvanize.it");
```
```
INSERT INTO student (name, phone_no, email)
VALUES ("Hunter Gillane", "567-1234", "hunter@galvanize.it");
```
Here is our movies table schema:


Our new row of data will look something like this:

```
id		name           	phone_no	email
0   	Spencer Eldred 	123-4567	spencer@galvanize.it
1		Hunter Gillane	567-1234	hunter@galvanize.it

```

Even though we did not specify an id, one was created anyways.  Since we have SERIAL as the data type of id , the database knows on a new insert to automatically assign a new unique id to the row.

## SELECT (Read part of CRUD)

A select statement allows you to get data from the database.  Here are the [SELECT doc](http://www.postgresql.org/docs/9.3/static/sql-select.html).  Also, postgres has a good [SELECT tutorial](http://www.postgresql.org/docs/9.3/static/tutorial-select.html).  
Given this table:

```
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone_no TEXT,
    email TEXT
);
```

```
SELECT * FROM students;
```

We may not want all attribues though.  Let's say instead we only care about the name and email of the student.  Here is the query we'd use:

```
SELECT name, email FROM students
```


This will select the name and email from students table where the name = "Spencer Eldred". 

```
SELECT name, email FROM students WHERE name = 'Spencer Eldred';
```

You can also have more complex queries to get data.  You can use boolean AND and OR operations in the selector: **WHERE name = 'Spencer Eldred' OR email = 'hunter@galvanize.it'**

```
SELECT name, email FROM students WHERE name = 'Spencer Eldred' OR email = 'hunter@galvanize.it';
```
You can order your results by using "ORDER BY": "DESC" - decending, "ASC" - ascending.

```
SELECT name, email FROM students ORDER BY name DESC;
```

Your list could get really long if there were many students. We can limit the size of the list by using "LIMIT".  Let's instead only get the top 5 movies that are returned using LIMIT:

```
SELECT name, email FROM students ORDER BY name DESC LIMIT 1;
```

## UPDATE (Update part of CRUD)

The update statement is defined [UPDATE doc](http://www.postgresql.org/docs/9.3/static/sql-update.html) in the postgres docs.  It is used to change existing data in our database.

For example, if we want to change the phone number for Spencer:

```
UPDATE students SET phone_no='222-1234' WHERE name='Spencer Eldred';
```

## DELETE (Destroy part of CRUD)

Deleting works similarly to a select statement.  Here are the [DELETE doc](http://www.postgresql.org/docs/9.3/static/sql-delete.html)

The statement below deletes a specific row from the database students (WHERE name='Spencer Eldred'):

```
DELETE FROM students WHERE name='Spencer Eldred';
```

