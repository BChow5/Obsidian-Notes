## Data Types

### Numeric data types
- integer or exact data types, decimal or approximate data types, and bit data types
- be default, REAL data types are stored as DOUBLE

#### Integer types
- MySQL also supports TINYINT, MEDIUMINT, and BIGINT

The following is the column declaration for the unsigned integer column:

```
CREATE TABLE employees
(salary INTEGER(5) UNSIGNED);
```

INT and INTEGER can be used interchangeably. But consider if we declared a column as:

```
CREATE TABLE employees
(id INT(255));
```

- The maximum value that an INTEGER column can store is either 2147483647 (in case of a signed INTEGER) or 4294967295 (in case of an unsigned INTEGER)
- ZEROFILL is an attribute which indicates that the number value should be prefixed with zeros if the length of the number value is smaller than the length of the column

#### Fixed point Types
- Fixed point types represent numbers with a fixed number of digits after the decimal or radix point
- MySQL has DECIMAL and NUMERIC as fixed point, or exact, value data types
	- stored in a binary format
- The value of a fixed point data type is an integer number scaled by a specific factor, according to the type
	- e.g. the value of 1.11 can be represented in fixed point as 111, with a scaling factor of 1/100

The following code block demonstrates the declaration of a DECIMAL data type:
```
CREATE TABLE taxes
(tax_rate DECIMAL(3, 2));
```
- The maximum number of digits supported for the DECIMAL type is 65, including precision and scale

#### Floating point types
- Floating point numbers represent real numbers in computing
- Real numbers are useful for measuring continuous values, such as weight, height, or speed
- two floating point data types for storing approximate values: **FLOAT** and **DOUBLE**

#### Bit value type
- The BIT data type is used to store binary bits or groups of bit values. It is also one of the options to store Boolean, yes/no or 0/1 values

The BIT type column can be defined as:
```
column_name BIT
or
column_name BIT(m)
where m = number of bits to be stored
```

bit literals can be written using the b'val' notation. There is another notation, which is the 0bval notation.

One important note about b'val' or 0bval notations is that the letter case of the leading b doesn't matter. We can specify b or B. A leading 0b is case-sensitive, and can't be replaced with 0B.

The following is the list of legal and illegal bit value literals.

Legal bit value literals:

- b'10'
- B'11'
- 0b10

#### Date and time data types
- DATE, TIME, DATETIME, TIMESTAMP, and YEAR form the group of date and time data types for storing temporal values
- The zero value can be 00-00-0000. MySQL allows this value to be stored in a date column. This is sometimes more convenient than storing NULL values
- Supplying a two-digit year creates ambiguity for MySQL to interpret the year because of the unknown century. The following are the rules that must be followed, using which MySQL interprets two-digit year values:
	- Year values between 70-99 are converted to 1970-1999
	- Year values between 00-69 are converted to 2000-2069
- TIMESTAMP data type is also suitable for values containing date and time parts
- NOW() is the function used to get the current date and time of the system
- The DATE() function is used to extract date information from the DATETIME value

#### String data type
- String data types are categorized into two categories: fixed length and variable length. CHAR, VARCHAR, BINARY, VARBINARY, BLOB, TEXT, ENUM, and SET are the MySQL-supported string data types

#### BLOB and TEXT data types
- **Binary Large Object** (**BLOB**)
- BLOB is MySQL's solution to storing large binary information of variable lengths. MySQL has four BLOB types: TINYBLOB, BLOB, MEDIUMBLOB, and LONGBLOB

### Storage Engines
- Storage engines are MySQL components for handling the SQL operations used in different types of tables
- MySQL storage engines are designed to manage different types of tasks in different types of environments
- The MySQL server has a pluggable storage engine architecture that enables storage engine loading as well as unloading from the already running MySQL server

#### InnoDB
- **InnoDB** is the default and most general-purpose storage engine in MySQL 8 and it is recommended by Oracle to use for tables as well as for special use cases
- If you have not configured a different default storage engine, then issuing the SQL statement CREATE TABLE without the ENGINE = clause creates a table with the storage engine InnoDB as the default engine in MySQL 8


#### types of storage engines
- **InnoDB:** The default storage engine for MySQL 8. It is an ACID compliant (transaction-safe) storage engine that has commit, roll back, and crash-recovery for protecting the user data and referential-integrity constraints to maintain data integrity, and much more.
- **MyISAM**: The storage engine with tables having a small footprint. It has table-level locking and so is mostly used in read-only or read-mostly data workloads, such as in data warehousing and web configurations.
- **Memory**: The storage engine previously known as the HEAP engine. It keeps data in RAM, which provides faster data access, mostly used in quick lookups of non-critical data environments.
- **CSV**: The storage engine with tables as comma-separated values in text files and tables. They are not indexed and are mostly used for importing and dumping data in CSV format.
- **Archive**: The storage engine comprises compact, unindexed tables, intended to store and retrieve a huge amount of historical, archived, or security audit data.
- **Blackhole**: The storage engine with tables that can be used for replication configuration. A query always returns an empty set. DML SQL statements are sent to slave servers. It accepts data but data is not stored, such as in a Unix /dev/null device use.
- **Merge**: The storage engine provides the capability to logically group a series of similar MyISAM tables and refer to them as one object instead of separate table.
- **Federated**: The storage engine that can link many separate physical MySQL servers into one logical database. It is ideal for data marts or distributed environments.
- **Example**: The storage engine that does nothing but works as a stub. It is primarily used by the developers who illustrate how to begin writing new storage engines in the MySQL source code.

### Indexing in MySQL
- To define an index on a table is the best way to improve the performance of the SELECT operation
- An index acts like a pointer for the table rows and permits queries to quickly point to matching rows based on the WHERE condition
- MySQL 8 allows you to create indexes on all the data types
- **Clustered index**: A clustered index defines the order in which data is physically stored in a table. Therefore, only one clustered index is allowed per table. It greatly increases the speed of retrieval when data is retrieved in a sequential manner, either in the same order or in reverse order. A clustered index also provides better performance when a range of items are selected. A primary key is defined as a clustered index.
- **Non-clustered index**: A non-clustered index doesn't define the order in which data is physically stored. This means a non-clustered index is stored in one place, and data is stored in another place. Therefore, more than one non-clustered index is allowed per table. It refers to non-primary keys.

#### uses of indexes
- Indexes are mainly used to find a row of specific values without iterating a complete table
- MySQL 8 uses indexes for the following operations:
	- To sort or group tables when it is done on the left-most prefix of an index. This means that if all keys are defined for the DESC clause, then the keys will be considered in reverse order, and if all keys are followed by ASC, then the keys will be considered in forward order.
	- To find rows whose values match with the WHERE clause.
	- In the case of multiple column indexes, any left-most prefix of the index can be used to find the row. This topic is covered in the later part of this chapter with a detailed example.
	- If there is a case where MySQL has to choose one index from multiple options, then it will choose the index which has the smallest set of rows.
	- Sometimes, the query is optimized to get the values without referring to rows. For example, if the query uses only columns that are included in indexes, MySQL 8 will get the selected value from the index tree:
-  If an index prefix exceeds its size, then MySQL 8 will handle the index as follows:
	- **For a non-unique index**: If the strict SQL mode is enabled, then MySQL 8 will throw an error, and if the strict mode is disabled, then the index length is reduced to the maximum column data type size and will produce a warning.
	- **For a unique index**: In this case, MySQL 8 produces an error regardless of the SQL mode, because it might break the uniqueness of the column. This means you have defined a column with 25 length and tried to define an index on the same column with a prefix length 27; then MySQL 8 throws an error.

#### Spatial index characteristics
MySQL 8 follows the following rules for spatial index characteristics:

- It is only available for InnoDB and MyISAM storage engines; if you try to use it for other storage engines, then MySQL 8 gives an error.
- A NULL value is not allowed for indexed columns.
- A prefix attribute is not allowed for this column. Full width will be considered for the index.

#### Non-spatial index characteristics

MySQL 8 follows the following rules for non-spatial index characteristics:

- A NULL value is allowed for an indexed column in the case of InnoDB, MyISAM, and MEMORY storage engines.
- The column prefix length must be specified in the case of each spatial column, if it exists under a non-spatial index. The prefix length will be considered in terms of bytes.
- Except for ARCHIVE, it is supported for all the storage engines which support spatial columns.
- A NULL value is allowed for this index, unless it is defined as a PRIMARY key.
- For an InnoDB table, run the ANALYZE TABLE statement after creating an index on that table, if the innodb_stats_persistent setting is enabled.
- The index type will depend on the storage engine; currently, B-Tree is used.
- A non-spatial index is allowed on a BLOB or TEXT column only if it is defined using InnoDB and MyISAM tables.

#### Creating an index
**Single-Column Index**
```
CREATE INDEX index_name ON table_name (column_name);
```

**Multi-Column (Composite) Index**
```
CREATE INDEX index_name ON table_name (column1, column2);
```

#### What is an Invisible Index?
An invisible index in MySQL is an index that exists in the database but is ignored by the query optimizer. This feature allows you to test whether an index is actually beneficial without fully dropping it.

creating invisible index
```
CREATE INDEX idx_salary ON employees (salary) INVISIBLE;
```

 **Making an Index Invisible**
`ALTER INDEX idx_salary INVISIBLE;`

**Making an Index Visible Again**
`ALTER INDEX idx_salary VISIBLE;`

**Why Use an Invisible Index?**
- To test the impact of removing an index without actually dropping it.
- To temporarily disable an index without affecting other operations.
- If a query performs better without an index, you can safely drop it later.