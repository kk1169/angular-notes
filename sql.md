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
