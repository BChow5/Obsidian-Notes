### Advantages of Using Stored Functions & Procedures in MySQL

#### 1. Performance Improvements

- **Reduces network traffic**: Since the logic runs inside the database, it minimizes the number of queries sent from the application.
- **Precompiled execution**: Stored procedures are compiled once and executed multiple times, improving performance for complex queries.
- **Efficient data processing**: Operations like aggregations and calculations are faster when executed close to the data source.

#### 2. Code Reusability & Maintainability

- **Encapsulation**: Business logic can be centralized in the database, reducing duplication across multiple application layers.
- **Easier updates**: Changes to procedures are applied at the database level without modifying the application code.
- **Better modularization**: Can be structured like functions in programming languages, making database logic easier to manage.

#### 3. Security & Access Control

- **Prevents SQL Injection**: Since stored procedures use **prepared statements**, they help mitigate injection attacks.
- **Fine-grained permissions**: Users can be given access to execute procedures **without direct table access**, improving security.

#### 4. Transaction Management

- **Consistency in transactions**: Stored procedures can handle **complex business logic** and **multiple SQL operations** within a single transaction.
- **Ensures atomicity**: Reduces the risk of partially executed operations that could corrupt data integrity.

### Disadvantages of Using Stored Functions & Procedures

#### 1. Debugging Complexity

- **Limited debugging tools**: Unlike application code, stored procedures in MySQL **lack robust debugging tools** (e.g., step-through debugging).
- **Harder to trace errors**: Error messages can be less descriptive compared to errors in application code.

#### 2. Scalability Issues

- **Difficult to scale horizontally**: Stored procedures increase CPU and memory usage on the database server, making it harder to distribute load across multiple database instances.
- **Not ideal for microservices**: Modern architectures favor stateless services, while stored procedures keep business logic inside the database, limiting flexibility.

#### 3. Maintainability Challenges

- **Tightly coupled logic**: Business logic inside the database can make applications harder to migrate to another database system.
- **Harder to version control**: Unlike application code, database-stored procedures require **manual version control** and tracking.

#### 4. Performance Bottlenecks

- **High database load**: If too many stored procedures run heavy processing, they can slow down the entire database.
- **Less efficient than application-side logic**: Some computations (e.g., JSON parsing, complex loops) are better handled by application code rather than SQL.

### Views
#### Create a View
```mysql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

#### Update a View
```mysql
CREATE OR REPLACE VIEW active_customers AS
SELECT id, name, email, status
FROM customers
WHERE status = 'active';
```

#### Drop a View
```mysql
DROP VIEW active_customers;
```

### Stored functions
- A **stored function** in MySQL is a reusable SQL **function** that returns a single value.
- Unlike **stored procedures**, stored functions **must return a value** and can be used inside SQL statements like `SELECT`
- a stored procedure will execute multiple SQL statements and perform tasks
	- cannot be in SQL queries directly
	- can modify data in the tables

#### Creating stored function
```mysql
DELIMITER $$

CREATE FUNCTION function_name (param1 DataType, param2 DataType, ...)
RETURNS ReturnType
DETERMINISTIC
BEGIN
    -- Function logic
    RETURN value;
END $$

DELIMITER ;
```

example
```mysql
DELIMITER $$

CREATE FUNCTION calculate_total(price DECIMAL(10,2), quantity INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN price * quantity;
END $$

DELIMITER ;
```

```mysql
SELECT id, price, quantity, calculate_total(price, quantity) AS total_cost
FROM orders;
```

#### Delete a stored function
```mysql
DROP FUNCTION IF EXISTS calculate_total;
```

### Trigger
- a trigger activates when a particular even occurs for the table 
	- cannot be done for views
- all triggers must have unique names within a schema
	- Triggers in different schemas can have the same name
- [`CREATE TRIGGER`](https://dev.mysql.com/doc/refman/8.0/en/create-trigger.html "15.1.22 CREATE TRIGGER Statement") requires the [`TRIGGER`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_trigger) privilege for the table associated with the trigger
- _`trigger_time`_ is the trigger action time. It can be `BEFORE` or `AFTER` to indicate that the trigger activates before or after each row to be modified
- _`trigger_event`_ indicates the kind of operation that activates the trigger. These _`trigger_event`_ values are permitted:
	- [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement"): The trigger activates whenever a new row is inserted into the table (for example, through [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement"), [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement"), and [`REPLACE`](https://dev.mysql.com/doc/refman/8.0/en/replace.html "15.2.12 REPLACE Statement") statements).
	- [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html "15.2.17 UPDATE Statement"): The trigger activates whenever a row is modified (for example, through [`UPDATE`](https://dev.mysql.com/doc/refman/8.0/en/update.html "15.2.17 UPDATE Statement") statements)
	- [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html "15.2.2 DELETE Statement"): The trigger activates whenever a row is deleted from the table (for example, through [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html "15.2.2 DELETE Statement") and [`REPLACE`](https://dev.mysql.com/doc/refman/8.0/en/replace.html "15.2.12 REPLACE Statement") statements). [`DROP TABLE`](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html "15.1.32 DROP TABLE Statement") and [`TRUNCATE TABLE`](https://dev.mysql.com/doc/refman/8.0/en/truncate-table.html "15.1.37 TRUNCATE TABLE Statement") statements on the table do _not_ activate this trigger, because they do not use [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html "15.2.2 DELETE Statement"). Dropping a partition does not activate [`DELETE`](https://dev.mysql.com/doc/refman/8.0/en/delete.html "15.2.2 DELETE Statement") triggers, either.
- The `DEFINER` clause specifies the MySQL account to be used when checking access privileges at trigger activation time.
	-  If the `DEFINER` clause is present, the _`user`_ value should be a MySQL account specified as ``'_`user_name`_'@'_`host_name`_'``, [`CURRENT_USER`](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_current-user), or [`CURRENT_USER()`](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_current-user).

#### Create trigger
```mysql
CREATE TRIGGER trigger_name 
{ BEFORE | AFTER } { INSERT | UPDATE | DELETE } 
ON table_name 
FOR EACH ROW 
BEGIN
    -- Trigger logic (SQL statements)
END;
```

example

```mysql
DELIMITER $$

CREATE TRIGGER before_salary_update
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (emp_id, old_salary, new_salary, change_date)
    VALUES (OLD.id, OLD.salary, NEW.salary, NOW());
END $$

DELIMITER ;
```

#### Abilities of triggers
- Automate database actions (logging, validation, auditing).
- Ensure data consistency (e.g., enforce business rules).
- Prevent accidental modifications (e.g., reject invalid changes).
- Improve security by controlling operations at the database level.

#### Limits of triggers
- Cannot call stored procedures directly inside a trigger.
- Triggers do not accept parameters (unlike stored procedures).
- Performance overhead since they execute for each row affected.
- Difficult to debug because they execute automatically and don't return errors visibly.
- No interactive operations (e.g., cannot SELECT data and return results).

#### View and delete triggers
```mysql
SHOW TRIGGERS;
```

```mysql
DROP TRIGGER IF EXISTS before_salary_update;
```