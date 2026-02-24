- Use the `secure_file_priv` variable to specify the directory for read and write permission. 
	- Using this variable, you can restrict non-administrative users from accessing important directories. Use this variable to set permissions on plugin_dir; it is very important. 
	- In the same way, do not provide FILE privileges to all the users, because it permits users to write files anywhere in the system.
- Use the `max_user_connections` variable to restrict the number of connections per account.

### Access Control in MySQL8
- Privileges are mainly used to authenticate users and will verify user credentials and check if a user is allowed for the requested operation or not
- User is not allowed to set a password on specific objects, such as a table or a routine
- As an admin user, we cannot specify privileges in a way that create/drop table is allowed but create/drop database of that table is not allowed

### Privileges provided by MySQL8
Privileges define which operations are permissible to the user accounts
- **Database privileges**: Applied on the database, and all objects of the database within it. It can be granted to a single database or defined globally to apply on all databases.
- **Administrative privileges**: It is defined at the global level, so not restricted to a single database. It enables users to manage operation of the MySQL 8 server.
- **Database object's privileges**: It is used to define privileges on the database objects, such as tables, views, indexes, and stored routines. It can be applied on a specific object of the database, can be applied on all objects of a given type in a database, or can be applied globally for all the objects of a given type in all databases.

Privileges are further classified in terms of static and dynamic privileges:
- **Static privileges**: These are available built in with the server and cannot be unregistered. These privileges are always available for the user to be granted.
- **Dynamic privileges**: These privileges can be registered or unregistered at runtime. If privileges are not registered, then they are not available to be granted for user accounts.

### Add and remove user accounts
MySQL 8 provides two different ways to create accounts:

- **Using account management statements**: These statements are used to create users and set their privileges; for example, with CREATE USER and GRANT statements, which inform the server to perform modifications on the grant table

- **Using manipulation of grant tables**: Using INSERT, UPDATE, and DELETE statements, we can manipulate the grant table

Out of these two approaches, account management statements are preferable, because they are more concise and less error-prone. Now, let's see an example of using the commands:

```
#1 mysql> CREATE USER 'user1'@'localhost' IDENTIFIED BY 'user1_password';
#2 mysql> GRANT ALL PRIVILEGES ON *.* TO 'user1'@'localhost' WITH GRANT OPTION;

#3 mysql> CREATE USER 'user2'@'%' IDENTIFIED BY 'user2_password';
#4 mysql> GRANT ALL PRIVILEGES ON *.* TO 'user2'@'%' WITH GRANT OPTION;

#5 mysql> CREATE USER 'adminuser'@'localhost' IDENTIFIED BY 'password';
#6 mysql> GRANT RELOAD,PROCESS ON *.* TO 'adminuser'@'localhost';

#7 mysql> CREATE USER 'tempuser'@'localhost';

#8 mysql> CREATE USER 'user4'@'host4.mycompany.com' IDENTIFIED BY 'password';
#9 mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON db1.* TO 'user4'@'host4. mycompany.com';

```
The preceding commands perform the following actions:
- #1 command creates 'user1' and command #2 assigns full privileges to 'user1'. But 'user1'@'localhost' indicates that 'user1' is allowed to connect with localhost only.
- #3 command creates 'user2' and command #4 assigns full privileges to 'user2', the same as 'user1'. But in #4, 'user2'@'%' is mentioned, which indicates that 'user2' is allowed to connect with any host.
- #5 creates 'adminuser' and allows it to connect with localhost only. In #6, we can see that only RELOAD and PROCESS privileges are provided to the 'adminuser'. It allows 'adminuser' to execute the mysqladmin reload, mysqladmin refresh, mysqladmin flush-xxx commands, and the mysqladmin processlist command, but it has no access on any database.
- #7 creates the 'tempuser' account without a password and allows the user to connect with localhost only. But no grant is specified for 'tempuser', so this user is not able to access the database nor perform any administrative commands.
- #8 creates 'user4' and allows the user to access the database using 'host4' only. #10 indicates 'user4' has grant on 'db1' for all the mention operations.
- 
To remove a user account, execute the DROP USER command as follows:
`mysql> DROP USER 'user1'@'localhost';`

### Create and Drop Role
CREATE ROLE is used to create a role; refer to the following command, which will create a role with the name 'developer_role':

`CREATE ROLE 'developer_role';`

`**DROP ROLE 'developer_role';**`

### Grant

GRANT assigns privileges to roles and assigns roles to accounts. For example, the following command assigns all privileges to the developer role:

`GRANT ALL ON my_db.* TO 'developer_role';`

In the same way, to assign roles to the user account, execute the following command:

`GRANT 'developer_role' TO 'developer1'@'localhost';`

### Revoke
REVOKE is used to remove privileges from the role and remove a role assignment from the user account. Refer to the following commands:

```
REVOKE developer_role FROM user1;
REVOKE INSERT, UPDATE ON app_db.* FROM 'role1';
```

### Set default role
SET DEFAULT ROLE indicates which roles are active by default, whenever user login default roles are available to the user. To set a default root, execute the following command:

```
mysql>SET DEFAULT ROLE app_developer TO root@localhost;

mysql> SELECT CURRENT_ROLE();
+---------------------+
| CURRENT_ROLE() |
+---------------------+
| `app_developer`@`%` |
+---------------------+
1 row in set (0.04 sec)
```

### # Encryption in MySQL 8
- MySQL 8 performs encryption per connection using **Transport Layer Security** (**TLS**) protocol.
- Make sure the encryption algorithm used contains security elements to protect your connection from known attacks, like changing the order of a message or replay twice on data

### Server-side configuration for encrypted connections

On the server side, MySQL 8 uses the –ssl option to specify properties related to encryption. The following options are used at the server side to configure encryption:

- `--ssl-ca`: This option specifies the path name of the **Certificate Authority** (**CA**) certificate file
- `--ssl-cert`: This option specifies the path name of the server public key certificate file
- `--ssl-key`: This option specifies the path name of the server private key file

You can use the above options by specifying them in the my.cnf file as follows:

```
[mysqld]  
ssl-ca=ca.pem  
ssl-cert=server-cert.pem  
ssl-key=server-key.pem
```

The --ssl option is enabled by default, so on server startup, MySQL 8 will try to find the certificate and key file under the data directory, even though you have not defined it in the my.cnf file

### Client-side configuration for encrypted connections
- At the client side, MySQL uses the same –ssl options used at the server side to specify the certificate and key file, but apart from that, it has –ssl-mode options

- `--ssl-mode=REQUIRED`: This option indicates that an encrypted connection must be established, and fails if it is not established
- `--ssl-mode=PREFFERED`: This option indicates the client program can establish an encrypted connection if the server permits it, or else establish an unencrypted connection without a fail
- `--ssl-mode=DISABLED`: This option indicates the client program is unable to use an encrypted connection, and only an unencrypted connection is allowed
- `--ssl-mode=VERIFY_CA`: This option is the same as REQUIRED, but in addition to that, it verifies the CA certificate against the configured CA certificate and returns a fail if no matches are found
- `--ssl-mode=VERIFY_IDENTITY`: It is the same as the VERIFY_CA option, but in addition to that, it will perform the hostname identity

### Command options for encrypted connections
The following options are available in MySQL 8 for an encrypted connection. You can use these options on the command line, or you can define them in an option file:

|   |   |
|---|---|
|**Format**|**Description**|
|--skip-ssl|Do not use encrypted connection|
|--ssl|Enable encrypted connection|
|--ssl-ca|File that contains a list of trusted SSL Certificate Authorities|
|--ssl-capath|Directory that contains trusted SSL Certificate Authority certificate files|
|--ssl-cert|File that contains X509 certificate|
|--ssl-cipher|List of permitted ciphers for connection encryption|
|--ssl-crl|File that contains certificate revocation lists|
|--ssl-crlpath|Directory that contains certificate revocation list files|
|--ssl-key|File that contains X509 key|
|--ssl-mode|Security state of connection to server|
|--tls-version|Protocols permitted for encrypted connections|