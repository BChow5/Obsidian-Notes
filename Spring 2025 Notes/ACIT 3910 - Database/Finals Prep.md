`ssh -i /path/to/private-key.pem username@VM-IP-Address`

### Create and Modify Views

#### Create
```mysql 
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name
WHERE condition;
```

```mysql
CREATE VIEW employee_salaries AS
SELECT first_name, last_name, salary
FROM employees
WHERE salary > 50000;
```
#### Modify
```mysql
CREATE OR REPLACE VIEW view_name AS
SELECT new_columns
FROM table_name
WHERE new_conditions;
```

```mysql
CREATE OR REPLACE VIEW employee_salaries AS
SELECT first_name, last_name, salary, department_id
FROM employees
WHERE salary > 60000;
```

#### Drop
```mysql
DROP VIEW view_name;
```

### Roles and Users

#### Create User
```mysql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
- Use `'%'` for any host: `'devuser'@'%'`.

#### Grant privilege 
```mysql
GRANT privileges ON database.table TO 'username'@'host';
```
- Common Privileges:
	- SELECT, INSERT, UPDATE, DELETE
	- ALL PRIVILEGES
	- CREATE, DROP, ALTER
	- GRANT OPTION (allows the user to grant privileges to others)

#### Create Role
```mysql
CREATE ROLE 'rolename';
```

#### Grant privileges to Role
```mysql
GRANT SELECT ON mydb.* TO 'readonly';
```

### Grant role to user 
```mysql
GRANT 'readonly' TO 'devuser'@'localhost';
```

**Activate the role**

```mysql
SET DEFAULT ROLE 'readonly' TO 'devuser'@'localhost';
```

### Backups with mysqldump and mysqlpump

#### mysqldump full backup (entire database)
```mysql
mysqldump -u [user] -p [database_name] > full_backup.sql
```

e.g.
```mysql
mysqldump -u root -p mydb > mydb_full_backup.sql
```

#### mysqldump partial backup (specific tables)
```mysql
mysqldump -u [user] -p [database_name] table1 table2 > partial_backup.sql
```

e.g.
```mysql
mysqldump -u root -p mydb customers orders > mydb_partial_backup.sql
```

#### mysqlpump full backup
```mysql
mysqlpump -u [user] -p [database_name] > full_backup_pump.sql
```

e.g.
```mysql
mysqlpump -u root -p mydb > mydb_full_backup_pump.sql
```

#### mysqlpump partial backup
```mysql
mysqlpump -u [user] -p [database_name] --tables table1 table2 > partial_backup_pump.sql
```

e.g.
```mysql
mysqlpump -u root -p mydb --tables customers orders > mydb_partial_backup_pump.sql
```

#### Restore backup
```mysql
mysql -u [user] -p [database_name] < backup.sql
```

e.g.
```mysql
mysql -u root -p mydb < mydb_full_backup.sql
```

### Import Comma-Separated Value file (CSV) data using LOAD DATA INFILE 
-  run: `SHOW VARIABLES LIKE "secure_file_priv";`
	- put the files into the folder that shows
```mysql
LOAD DATA INFILE '/var/lib/mysql-files/data.csv'
INTO TABLE users
FIELDS TERMINATED BY ','  
ENCLOSED BY '"'  
LINES TERMINATED BY '\n'  
IGNORE 1 ROWS;
```

- **`FIELDS TERMINATED BY ','`** → Defines the column separator (comma in this case).
- **`ENCLOSED BY '"'`** → Specifies that text fields are enclosed in double quotes.
- **`LINES TERMINATED BY '\n'`** → Defines new rows as newline-separated.
- **`IGNORE 1 ROWS`** → Skips the first row (header row).

#### LINES TERMINATED BY
check `file data.txt` to see what kind of text file this is for lines terminated by

- Windows line terminators (CRLF) - `\r\n`
- Linux line terminators (LF) - `\n`
- Mac line terminators (CR) - `\n`
- can check what kind the csv is on Linux by doing `file data.csv`

Hint: if using LOAD DATA INFILE, you may get an error if you don't use the correct file location to load from. Error: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement. To see which location is allowed to be imported from run: `SHOW VARIABLES LIKE "secure_file_priv";` Put your file here and try again. Do NOT disable the secure_file_priv option.

Hint 2: When specifying the filename and path, make sure to properly escape the backslash character "\" with 2 backslashes like this "\\". For example, if I wanted to use: C:\temp\words.txt I would specify it as such: "C:\\temp\\words.txt" (each backslash needs to be escaped with a 2nd backslash). 

### Create Stored Procedure 
```mysql
DELIMITER //

CREATE PROCEDURE procedure_name (IN param1 INT, OUT param2 VARCHAR(50))
BEGIN
    -- your SQL logic here
    SELECT some_column INTO param2 FROM some_table WHERE id = param1;
END //

DELIMITER ;
```

call it
```mysql
CALL GetAllUsers();
```

drop
```mysql
DROP PROCEDURE IF EXISTS GetUserById;
```

### create stored function
```mysql
DELIMITER //

CREATE FUNCTION function_name (param1 DATATYPE, param2 DATATYPE)
RETURNS return_datatype
DETERMINISTIC
BEGIN
    -- Declare variables if needed
    DECLARE result return_datatype;

    -- Do some logic
    SET result = param1 + param2;

    -- Return the result
    RETURN result;
END //

DELIMITER ;
```

```mysql
DROP FUNCTION IF EXISTS AddNumbers;
```

### Create scheduled event

Run this to check:

```mysql
SHOW VARIABLES LIKE 'event_scheduler';
```

If it's `OFF`, turn it on:
`SET GLOBAL event_scheduler = ON;`


```mysql
CREATE EVENT IF NOT EXISTS delete_old_logs
ON SCHEDULE EVERY 1 DAY
STARTS CURRENT_TIMESTAMP
DO
  DELETE FROM logs WHERE created_at < NOW() - INTERVAL 30 DAY;
```

### Create Triggers
```mysql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- your SQL statements here
END;
```

### Explain to optimize query
```mysql
EXPLAIN SELECT * FROM users WHERE email = 'user@example.com';
```

## 🧠 What `EXPLAIN` Tells You

The output includes:

|Column|Meaning|
|---|---|
|`id`|Query order (esp. in JOINs or subqueries)|
|`select_type`|Type of SELECT (simple, subquery, etc.)|
|`table`|Table being accessed|
|`type`|Join type — **lower = better** (e.g., `const`, `ref`, `ALL`)|
|`possible_keys`|Indexes MySQL _might_ use|
|`key`|Index MySQL _actually_ uses|
|`rows`|Estimated rows to scan|
|`Extra`|Extra info (e.g., `Using index`, `Using where`, `Using filesort`)|

#### Create Index
```mysql
CREATE INDEX idx_email ON users(email);
```

### Summary table

#### create summary table
```mysql
CREATE TABLE user_order_summary (
    user_id INT PRIMARY KEY,
    order_count INT,
    total_spent DECIMAL(10, 2)
);
```

#### fill summary table
```mysql
INSERT INTO user_order_summary (user_id, order_count, total_spent)
SELECT user_id, COUNT(*), SUM(total)
FROM orders
GROUP BY user_id;
```