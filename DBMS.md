

## START 

## install
```
sudo apt-get install mysql-server
```

## open mysql

```
sudo mysql
```

# Show database

```
show databases;
```
# Create database
```
create database databasename;
```

``
databasename as your database name
``

# to use database
```
use databasename;
```

# Create tables 

```
create table tablename (insert_value_method);
```

[insert_value_method](#insert_value_method)

# insert a new value
```
INSERT INTO table_name(column_1,column_2,column_3) 
VALUES (value_1,value_2,value_3); 
```

# When inserting multiple rows into a MySQL table, the syntax is as follows

```
INSERT INTO table_name (column_1, column_2, column_3, ...) 
VALUES 
      (value_list_1), 
      (value_list_2), 
      ...
      (value_list_n); 
```


# MySQL INSERT Examples
## Let’s explore some examples of using a MySQL INSERT statement when performing data-related tasks.

## To start with, create three tables: Customers, Products01, and Smartphones by executing the following script:

```
CREATE TABLE Customers
(
  ID int,
  Age int,
  FirstName varchar(20),
  LastName varchar(20)
);

-- Creating the Products table

CREATE TABLE Products01
(
  ID int,
  ProductName varchar(30) NOT NULL,
  Manufacturer varchar(20) NOT NULL,
  Price decimal NOT NULL,
  ProductCount int DEFAULT 0
);

CREATE TABLE Smartphones
(
  ID int,
  ProductName varchar(30) NOT NULL,
  Manufacturer varchar(20) NOT NULL,
  Price decimal NOT NULL,
  ProductCount int DEFAULT 0
);
```

# insert_value_method

insert a character
```
varchar(255)
```
insert a number
```
int(255)
```

