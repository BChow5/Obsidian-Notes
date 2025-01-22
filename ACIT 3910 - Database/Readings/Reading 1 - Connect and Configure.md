### Chapter 1 
- MySQL is a well-known open source structured database because of its performance, easiness to use, and reliability.
- **Structured Query Language** (**SQL**) is used to manipulate, retrieve, insert, update, and delete data in **relational database management system** (**RDBMS**).
- SQL is a standard language that all RDBMS systems such as MySQL, MS Access, MS SQL, Oracle, Postgres, and others use.
- As a relational database, MySQL has capabilities to establish relationships with different tables such as one to many, many to one, and one to one by providing primary keys, foreign keys, and indexes.'
- The MySQL server is covered under the **General Public License** (**GNU**), which means that we can freely use it for web applications, study its source code, and modify it to suit our needs.

#### Database Storage Engines and Types 
- The storage engine is the way to handle SQL operations for different table types
- MySQL stores the table definition in .frm with the same name as the table name_._ You can use the SHOW TABLE STATUS command to show information about your table
- The MySQL server uses a plug-and-play storage engine architecture. You can load the required storage engine and unload unnecessary storage engines from the MySQL server with the help of the SHOW ENGINES command
- InnoDB is the default storage engine broadly used out of all other available storage engines

#### benefits of MySQL8
- InnoDB is the default storage engine when we create a new table in MySQL 8.
- security 
	- MySQL is the most secure and reliable database management system used by many well-known enterprises
	- Roles can also be defined with a list of permissions that can be granted or revoked for the user. 
	- All user passwords are stored in an encrypted format using plugin-specific algorithms.
- scalability 
	- MySQL is a rewarding database system for its scalability, which can scale horizontally and vertically; in terms of data, spreading database and load of application queries across multiple MySQL servers is quite feasible
- open source
- high performance

#### What is new in MySQL8?
- **transaction data dictionary**
	- MySQL data dictionary was stored in different metadata files and non-transactional tables, but from this version onwards, it will have a transactional data dictionary to store the information about the database
	- All information will be stored in the database, which removes the cost of performing heavy file operations
	- improved performance as this data dictionary object can be cached in memory, similar to other database objects
- **roles**
	- roles are a collection of permissions
	- can now create roles with a number of privileges and assign them to multiple users
	- just need to make any privilege changes in the role and all users will automatically inherit the respective privileges
	- Roles can be created, deleted, grant or revoke permission, grant or revoke from the user account, and can specify the default role within the current session
- **InnoDB auto increment** 
	- now the auto-increment counter value is written into the redo log whenever the value gets changed and, on each checkpoint, it will be saved in the system table, which makes it persistent across the server restart
- **invisible indexes**
	- invisible indexes cannot be used by the optimizer
	- In case you want to test the query performance without indexes, using this feature you can do so by making them invisible rather than dropping and re-adding an index
- **improving descending indexes**
	- scans descending indexes in forward order
	- brings multiple column indexes for the optimizer when the most efficient scan order has ascending order for some columns, and descending order for other columns
- **The SET PERSIST variant**
	- Server variables can be configured globally and dynamically while the server is running
	- the SET PERSIST variant, which preserves variables across a server restart
	- `SET PERSIST max_connections = 1000;`
- **Expanded GIS support**
	- added support for a **Spatial Reference System** (**SRS**) with geo-referenced ellipsoids and 2D projections
	- SRS helps assign coordinates to a location and establishes relationships between sets of such coordinates
	- This spatial data can be managed in data dictionary storage as the ST_SPATIAL_REFERENCE_SYSTEMS table
- **Default character set**
	- changed from latin1 to UTF8
- **Extended bit-wise operations**
	- improved bit-wise operations by enabling support for other binary data types such as Binary, VarBinary, and BLOB
	- makes it possible to perform bit-wise operations on larger than 64-bit data
- **InnoDB Memcached**
	- Now, multiple key value pairs can be fetched in a single memcached query
- **NOWAIT and SKIP LOCKED**
	- To avoid waiting for the other transaction, InnoDB has added support of the NOWAIT and SKIP LOCKED options
	- NOWAIT will return immediately with an error in case the requested row is locked rather than going into the waiting mode, and SKIP LOCKED will skip the locked row and never wait to acquire the row lock
- **JSON**
	- JSON support had been implemented in MySQL 5.7
	- MySQL 8 it has added various functions that would allow us to get dataset results in JSON data format, virtual columns, and tentatively 15 SQL functions that allow you to search and use JSON data on server side
	- additional aggregation functions added that can be used in JSON objects/arrays to represent loaded data in a further optimized way
	- The following are the two JSON aggregation functions that were introduced in MySQL8:
		- JSON_OBJECTAGG()
		- JSON_ARRAYAGG()
- **Cloud** 
	- new option is introduced innodb_dedicated_server, which would be helpful for vertical scaling of the servers
	- automatically detects the memory allocated to the virtual server and appropriately set MySQL 8 without any need to change configuration files
- **Resource management**
	- wonderful resource management feature that will allow you to allocate resource to threads running on a server, which would be executed based on the resources configured for the group
	- Currently, CPU time is a resource that can be configured for a group

### Chapter 2 Installation layout 

#### Where for MySQL8 put data and configuration files for Windows and Linux?
- windows
	- **Data Directory (Default):**
		- `C:\ProgramData\MySQL\MySQL Server 8.0\Data\`
			- Stores databases, logs, and other related data.
	- **Configuration File (my.ini):**
		- `C:\ProgramData\MySQL\MySQL Server 8.0\my.ini`
			- Contains configuration settings like port, buffer sizes, and paths.
-  Linux (e.g., Ubuntu):
    - **Data Directory (Default)**:
        - `/var/lib/mysql/`
            - Stores databases, logs, and other related data.
    - **Configuration File (my.cnf)**:
        - `/etc/mysql/my.cnf` (or `/etc/my.cnf` on some distributions)
            - Contains configuration settings, including database paths and server options.

### Chapter 3 SQL Command Line
- Arguments beginning with single or double dash (-,--) are used for specifying the program options
- enter the program name followed by options or any other arguments required to tell the program what you want it to do
- For connecting to the server program, we need some information to specify the hostname, username, and the password of the MySQL account because we need to tell the client program which host the server is running in and the associated username and password
- `mysql --host=localhost --user=root --password=mypwd mysampledb`
- `mysql`
	- applies defaults for hostname, username, and no password, and no default database
- Providing the options on the command line followed by the program name
  The option name is case sensitive
- Option arguments begin with a single dash or double dashes, depending on if they are using the short form or the longer form of the option name
- Options can also take a value followed by the option name
- some examples
	- group
		- group is the name of the program or group for which options are to be set
	- opt_name
		- similar to the --opt_name in the command line turning on the named optimization
	- opt_name=value
		- similar to the --opt_name in the command line but in place of the value, you can specify the value with spaces, which you cannot in the command line
- In Command Prompt, we can set environment variables which will affect the runtime execution of the program, or can be set to affect future executions permanently
	- `MYSQL_TCP_PORT=3306`   
	- `export MYSQL_TCP_PORT`
- `mysqldump` is a command-line utility provided by MySQL for creating logical backups of databases
	- It exports databases into SQL files that contain the statements required to recreate the database schema, tables, and optionally, the data itself
	- `mysqldump -u [username] -p [database_name] > [backup_file.sql]`
	- restore the backup: `mysql -u [username] -p [database_name] < [backup_file.sql]

### Chapter 5 MySQL 8 Database Management
- allowed to change the **option file** by removing or adding a comment sign (#) at the start of the line of a specific parameter setting

#### Server options and different types of variables
- **Server option**: As described in the previous chapter MySQL 8 uses the option file and command line arguments to set startup parameters
- **Server System variable**: The MySQL server manages many system variables.
	- MySQL provides the default value for each system variable
	- System variables can be set using the command line or can be defined in the option file
	- can change these variables at runtime without server start or stop
- **Server status variable**: The MySQL server uses many status variables to provide information about its operation

#### Server SQL modes
- MySQL 8 provides different modes that will affect MySQL support and data validation checks
- makes it easier for the user to use MySQL in different environments
- MySQL provides the sql_mode system variable which can be set at either a global or session level

#### Setting the SQL mode
- SQL mode can be setup on startup using the `--sql-mode="modes"` option
- The user can also define this option in the option file as `sql-mode="modes"`

To change mode at runtime, execute the following commands:
```
SET GLOBAL sql_mode = 'modes';
SET SESSION sql_mode = 'modes';
```

To retrieve the values of both the variables, execute the following commands:
```
SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;
```

#### Available SQL modes
- **ANSI:** This mode is used to change syntax and behavior, by making it closer to standard SQL.
- **STRICT_TRANS_TABLES**: As the name implies, this mode is related to transaction and it is mainly used for transactional storage engines. 
	- When this mode is enable for nontransactional tables, MySQL 8 will convert invalid values to the closest valid value and insert the adjusted value into the column. 
	- If the value is missing, then MySQL 8 will insert an implicit default value related to the column's data type. In this case, MySQL 8 will generate a warning message instead of an error message, and continue with the statement execution without breaking it.
	- In the case of transactional tables, however, MySQL 8 gives an error and will breaks execution.
- **TRADITIONAL**: This mode generally behaves like traditional SQL database system. It indicates give error instead of a warning when an incorrect value inserted into the column.
- there are other options but these 3 are the most important SQL modes

#### Strict SQL mode
- **strict mode** is used to manage _Invalid data_ or _missing data_
- If strict mode is not enabled, then MySQL will manage the insert and update operations by adjusting values and generating warning messages
- We can do the same on strict mode by enabling the INSERT IGNORE or UPDATE IGNORE options
- Strict mode affects handling of divisions by zero, zero dates, and zeros in dates as describe in the preceding points with the `ERROR_FOR_DIVISION_BY_ZERO`, `NO_ZERO_DATE` and `NO_ZERO_IN_DATE` modes.

#### Ignore keyword
- The IGNORE keyword is used to downgrade errors to warnings and applicable to several statements
- For multiple row statements, the IGNORE keyword allows you to skip the particular row, instead of aborting
- The following statements support the IGNORE keyword:
	- CREATE TABLE ... SELECT: Individual CREATE and SELECT statements do not have support on this keyword, but when we insert into the table using SELECT statement, rows that duplicate an existing row on a unique key value are discarded.
	- DELETE: If this statement executes with the IGNORE option MySQL avoid errors occurred during execution.
	- INSERT: Duplicate values in unique key and data conversion issues will be handled by this keyword during row insertion. MySQL will insert the closest possible values into the column and ignore the error.
	- LOAD DATA and LOAD XML: At the time of loading data if duplication is found the statement will discard it and continue insertion for the remaining data if the IGNORE keyword is defined.
	- UPDATE: In cases of duplicate key conflict on unique key during statement execution, MySQL will update the column with the closest identified values.

#### IPv6 support
MySQL 8 provides support for **IPv6**, with the following capabilities:
- MySQL server will accept TCP/IP connections from clients with IPv6 connectivity
- MySQL 8 account names permit IPv6 addresses, which enables DBA to specify privileges for the clients that are connected with server, using IPv6
- The IPv6 functions enable conversions between string and internal IPv6 address formats, and checking whether the values represent a valid IPv6 address or not

### Chapter 15
#### How to reset the root password
**The following are the instructions to reset the root @ localhost account password on the Windows system:**

1. Log in to the system using system administrator credentials.
2. If the MySQL server is already running, stop the server. If the MySQL server is running as a Windows service, go to **Services** by following Start menu | Control panel | Administrative tools | Services. In the services, find the MySQL service and stop it. If the MySQL server is not running as a Windows service, kill the MySQL server process by using Windows Task Manager.
3. Once the MySQL server is stopped, create a text file that has a single line of the password assignment statement, as follows:

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';
```

4. Save the file. For example, the file is saved as C:\mysql-root-reset.txt.
5. Open the Windows Command Prompt by following Start menu | Run | cmd.
6. In the Command Prompt, start the MySQL server with the --init-file option, as follows:
```
C:\> cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"   
C:\> mysqld --init-file=C:\\mysql-root-reset.txt
```
1. Once the MySQL server is restarted, delete theC:\mysql-root-reset.txtfile.


**The following are the instructions to reset the root user password on Unix-like systems:**

1. Log on to the system with the same user the the MySQL server runs by. Usually, it is mysql user.
2. If the MySQL server is already running, stop the server. To accomplish this, find the .pid file containing the process ID of the MySQL server. Depending on the Unix distribution, the actual location and name of the file may differ. The usual locations are /var/lib/mysql/, /var/run/mysqld/, and /usr/local/mysql/data/. Usually, the filename begins with either mysqld or the hostname of the system and has an extension of .pid. The MySQL server can be stopped by sending a normal kill command to the mysqld server process. The following command can be used with actual path name of the .pid file:

        **> kill 'cat /mysql-data-directory/host_name.pid'**

3. Once the MySQL server is stopped, create a text file that has a single line of the password assignment statement, as follows:

        **ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';**

4. Save the file. It is assumed that the file is stored at /home/me/mysql-reset-root. As the file contains the password for the root user, it should be ensured that other users are not able to read it. If we are not logged in with the appropriate user, we should make sure that the user has permission to read the file.
5. Start the MySQL server with the --init-file option, as follows:

        **> mysqld --init-file=/home/me/mysql-reset-root &**

6. Once the server is started, delete the file at /home/me/mysql-reset-root.
`
