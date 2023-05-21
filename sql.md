# SQL COMMANDS

**CREATE DATABASE**
```
CREATE DATABASE <database_name>
```

**SELECT DATABASE**
```
USE test_app;
```
**CREATE TABLE**
```
CREATE TABLE users(
	id int(11) PRIMARY KEY auto_increment,
    name varchar(255),
    email varchar(255),
    mobile varchar(255),
    created_at datetime
)
```
**INSERT DATA INTO TABLE**
```
INSERT INTO table_name(column1, column2, ....)
VALUES(value1, value2, ....)
```
**SELECT QUERY**
```
SELECT * FROM <table_name>;
```
**UPDATE QUERY**
```
UPDATE users SET mobile='7744558855',email='' WHERE id=2;
```
**DELETE QUERY**
```
DELETE FROM users where id=4
```

## SELECT EXAMPLES

**CONCAT(column1,' ',column2)**
```
SELECT firstName, lastName, concat(firstName,' ',lastName) as fullName 
FROM employees;
```
**ORDER BY**
```
SELECT * FROM employees ORDER BY employeeNumber DESC;
```
**ORDER BY FIELD**
```
SELECT orderNumber, status FROM orders
ORDER BY field(status, 'Cancelled', 'Resolved','Shipped','In Process','On Hold','Disputed')
```
**FOREIGN KEY**
```
CREATE TABLE test_app.orders(
	orderId int(11) PRIMARY KEY auto_increment,
    orderName varchar(255),
    user_id int(11), FOREIGN KEY (user_id) REFERENCES users(id) 
)
```
