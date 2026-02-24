`ssh -i /path/to/private-key.pem username@VM-IP-Address`

### Change the MySQL Port (on Windows and Linux)  
#### Windows
- in notepad, find `my.ini` in `C:\Program Data\MySQL\MySQL Server 8.0\`
- find `[mysqld]` section change the port to whatever you want
- restart MySQL80 to apply changes

#### Linux
- `sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`
- change the port number
- `sudo service mysql restart`

### Login to MySQL using a non-standard port  
- `mysql -u username -p -P port_number -h host_name`
	- `-u` specifies the **username** (e.g., `root`).
	- `-p` prompts you to enter the **password**.
	- `-P` specifies the **port number** (e.g., `3307` if you’ve changed it).
	- `-h` specifies the **host** (e.g., `localhost` or an IP address).

### Reset the MySQL root password (on Windows and Linux)
#### Windows
- Stop MySQL Windows Service
- Create a text file called changeRootPassword.sql and save to c:\temp\ with the following contents (create the \temp\ directory if it doesn't exist):
	- `ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPassword';`
		- Replace 'MyNewPassword' with your new root password.
- Open a windows command (CMD.exe) line as an administrator and execute the following command: 
	- `mysqld --defaults-file="C:\ProgramData\MySQL\MySQL Server 8.0\my.ini" --init-file=C:\temp\ChangeRootPassword.sql`
	- After giving it 10 seconds to load and run your query in ChangeRootPassword.sql. 
	- You’ll have to abort it by pressing CTRL+C.
- Restart MySQL80 Windows Service.
- Delete your file (C:\temp\ChangeRootPassword.sql) when done.

#### Linux
- Stop SQL service
	- `sudo systemctl stop mysql`
- We are going to make a directory so that mysql doesn't complain about it not existing in the next step. 
	- `sudo mkdir /var/run/mysqld/`
- And give mysql permission to access it: 
	- `sudo chown mysql /var/run/mysqld/`
- `sudo mysqld_safe --skip-grant-tables --skip-networking --user=mysql &`
- Login to Mysql using just: `mysql`
- Run this MySQL command to initialize the privileges `FLUSH PRIVILEGES;`
- `ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPassword';`
	- Replace 'MyNewPassword' with your new root password.
- `FLUSH PRIVILEGES;`
- quit MySQL
- `sudo killall mysqld`
	- to stop MySQL running in safe mode
	- anyone can login without a password if left in safe mode 
- `sudo service mysql start`

### Create databases
	- `CREATE DATABASE my_database_name;`
- `USE my_database_name;`
- `SHOW DATABASES;`

### Tables 

#### Create Tables
```mysql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100),
	department_id INT,
	FOREIGN KEY (department_id) REFERENCES department(department_id) ON DELETE CASCADE
);
```
- `SHOW TABLES;`

#### Insert 
```mysql
INSERT INTO table_name (column1, column2, column3, ...) 
VALUES (value1, value2, value3, ...);
```

#### Update
- used for updating already existing data
```mysql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

example
```mysql
UPDATE users
SET email = 'alice.newemail@example.com'
WHERE id = 1;
```

#### Alter
- used to modify table structure (columns, indexes, constraints, etc.)
```mysql
ALTER TABLE employee
ADD CONSTRAINT fk_department
FOREIGN KEY (department_id)
REFERENCES department(department_id)
ON DELETE CASCADE;
```

#### Delete
```mysql
DELETE FROM table_name
WHERE condition;
```

example
```mysql
DELETE FROM users
WHERE id = 1;
```

### Create MySQL users, roles and grant them privileges  

#### Create User
```mysql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'userpassword';
```

#### Create Role
```mysql
CREATE ROLE 'role_name';
```
#### Grant

**Grant privileges to User**
```mysql
GRANT SELECT, INSERT ON my_database.* TO 'newuser'@'localhost';
```

**Grant privileges to Role**
```mysql
GRANT SELECT ON my_database.* TO 'read_only_role';
```

**Check grants**
```mysql
SHOW GRANTS for Sally@'10.0.0.5';
```

#### Assign Role to User
```mysql
GRANT 'role_name' TO 'username'@'host';
```

**Activate their roles**
```mysql
SET DEFAULT ROLE ALL TO 'Helen';
```

#### Revoke
```mysql
REVOKE privilege_type ON database_name.table_name FROM 'username'@'host';
```

example
```mysql
REVOKE INSERT ON my_database.* FROM 'newuser'@'localhost';
```

#### Drop
```mysql
DROP USER 'username'@'host';
```

```mysql
DROP ROLE 'role_name';
```

#### Check Users
```mysql
SELECT user, host FROM mysql.user;
```

Troubleshoot users and privileges  
(i.e. fix Susanna's privileges so she can access these tables)  

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
- Windows line terminators (CRLF) - `\r\n`
- Linux line terminators (LF) - `\n`
- Mac line terminators (CR) - `\n`
- can check what kind the csv is on Linux by doing `file data.csv`

Hint: if using LOAD DATA INFILE, you may get an error if you don't use the correct file location to load from. Error: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement. To see which location is allowed to be imported from run: `SHOW VARIABLES LIKE "secure_file_priv";` Put your file here and try again. Do NOT disable the secure_file_priv option.

Hint 2: When specifying the filename and path, make sure to properly escape the backslash character "\" with 2 backslashes like this "\\". For example, if I wanted to use: C:\temp\words.txt I would specify it as such: "C:\\temp\\words.txt" (each backslash needs to be escaped with a 2nd backslash). 


### Import SQL Formatted data from existing mysqldump file  
- you will need to manually create the database first if not included in the dump file 

```mysql
mysql -u root -p mydatabase < ~/mysqldump.sql
```

### Recover from an accidental delete of data within a database.  
- check the 2 tables to see which info is missing 
```mysql
SELECT b.*
FROM employees_backup.employees b
LEFT JOIN employees.employees e 
ON b.emp_no = e.emp_no
WHERE e.emp_no IS NULL;
```
- then insert the missing data (assuming the tables have the same structure)
```mysql
INSERT INTO employees.employees  
SELECT *  
FROM employees_backup.employees b  
LEFT JOIN employees.employees e  
ON b.emp_no = e.emp_no  
WHERE e.emp_no IS NULL;
```

**If tables have different structure** 
```mysql
INSERT INTO employees.employees (emp_no, first_name, last_name, gender, hire_date)
SELECT b.emp_no, b.first_name, b.last_name, b.gender, b.hire_date
FROM employees_backup.employees b
LEFT JOIN employees.employees e 
ON b.emp_no = e.emp_no
WHERE e.emp_no IS NULL;
```
- we would specify which columns we want data from and where to put it