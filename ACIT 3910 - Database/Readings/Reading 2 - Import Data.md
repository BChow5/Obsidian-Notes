### Insert Statement
- [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") inserts new rows into an existing table.
	- The [`INSERT ... VALUES`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement"), [`INSERT ... VALUES ROW()`](https://dev.mysql.com/doc/refman/8.0/en/values.html "15.2.19 VALUES Statement"), and [`INSERT ... SET`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") forms of the statement insert rows based on explicitly specified values
- The [`INSERT ... SELECT`](https://dev.mysql.com/doc/refman/8.0/en/insert-select.html "15.2.7.1 INSERT ... SELECT Statement") form inserts rows selected from another table or tables
- You can also use [`INSERT ... TABLE`](https://dev.mysql.com/doc/refman/8.0/en/table.html "15.2.16 TABLE Statement") in MySQL 8.0.19 and later to insert rows from a single table
-  [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") with an `ON DUPLICATE KEY UPDATE` clause enables existing rows to be updated if a row to be inserted would cause a duplicate value in a `UNIQUE` index or `PRIMARY KEY`
- Inserting into a table requires the [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_insert) privilege for the table
- The `PARTITION` clause takes a list of the comma-separated names of one or more partitions or subpartitions (or both) of the table
	- If any of the rows to be inserted by a given [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") statement do not match one of the partitions listed, the [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") statement fails with the error
- If strict SQL mode is not enabled, any column not explicitly given a value is set to its default (explicit or implicit) value
	- If strict SQL mode is enabled, an [`INSERT`](https://dev.mysql.com/doc/refman/8.0/en/insert.html "15.2.7 INSERT Statement") statement generates an error if it does not specify an explicit value for every column that has no default value
- `INSERT INTO tbl_name (col1,col2) VALUES(15,"Hello");`
	- inserting in a table named tbl_name
	- specified the columns (col1 and col2) where the values will be inserted into
	- values provides the values that will be inserted into 

###  LOAD DATA INFILE
- The [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") statement reads rows from a text file into a table at a very high speed
	- file can be read from the server host or the client host, depending on whether the `LOCAL` modifier is given
		- with local, file is read from client host (i.e., the machine running the MySQL client or application)
		- without local, it is read from server host
	- **Without `LOCAL`**: Used when the file is stored securely on the server and should not leave the server's environment.
	- **With `LOCAL`**: Used when the file resides on the client's local machine and needs to be uploaded to the server.
- [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") is the complement of [`SELECT ... INTO OUTFILE`](https://dev.mysql.com/doc/refman/8.0/en/select-into.html "15.2.13.1 SELECT ... INTO Statement")
	- To write data from a table to a file, use [`SELECT ... INTO OUTFILE`](https://dev.mysql.com/doc/refman/8.0/en/select-into.html "15.2.13.1 SELECT ... INTO Statement").
	- To read the file back into a table, use [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement")
- The `LOCAL` modifier affects these aspects of [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement"), compared to non-`LOCAL` operation:
	- It changes the expected location of the input file; see [Input File Location](https://dev.mysql.com/doc/refman/8.0/en/load-data.html#load-data-file-location "Input File Location").
	- It changes the statement security requirements; see [Security Requirements](https://dev.mysql.com/doc/refman/8.0/en/load-data.html#load-data-security-requirements "Security Requirements").
	- Unless `REPLACE` is also specified, `LOCAL` has the same effect as the `IGNORE` modifier on the interpretation of input file contents and error handling; see [Duplicate-Key and Error Handling](https://dev.mysql.com/doc/refman/8.0/en/load-data.html#load-data-error-handling "Duplicate-Key and Error Handling"), and [Column Value Assignment](https://dev.mysql.com/doc/refman/8.0/en/load-data.html#load-data-column-assignments "Column Value Assignment").
	- `LOCAL` works only if the server and your client both have been configured to permit it

#### Input File Character Set
- The file name must be given as a literal string
- By default, the server interprets the file contents using the character set indicated by the [`character_set_database`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_character_set_database) system variable
- [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") interprets all fields in the file as having the same character set, regardless of the data types of the columns into which field values are loaded

#### Input File Location
These rules determine the [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") input file location:
- If `LOCAL` is not specified, the file must be located on the server host. The server reads the file directly, locating it as follows:
    - If the file name is an absolute path name, the server uses it as given.
    - If the file name is a relative path name with leading components, the server looks for the file relative to its data directory.
    - If the file name has no leading components, the server looks for the file in the database directory of the default database.
- If `LOCAL` is specified, the file must be located on the client host. The client program reads the file, locating it as follows:
    - If the file name is an absolute path name, the client program uses it as given.
    - If the file name is a relative path name, the client program looks for the file relative to its invocation directory.
- `LOAD DATA INFILE 'data.txt' INTO TABLE db2.my_table;`

#### Field and Line Handling
- For both the [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") and [`SELECT ... INTO OUTFILE`](https://dev.mysql.com/doc/refman/8.0/en/select-into.html "15.2.13.1 SELECT ... INTO Statement") statements, the syntax of the `FIELDS` and `LINES` clauses is the same
	- Both clauses are optional, but `FIELDS` must precede `LINES` if both are specified.
- If you specify a `FIELDS` clause, each of its subclauses (`TERMINATED BY`, `[OPTIONALLY] ENCLOSED BY`, and `ESCAPED BY`) is also optional, except that you must specify at least one of them
- If you specify no `FIELDS` or `LINES` clause, the defaults are the same as if you had written this:
	- `FIELDS TERMINATED BY '\t' ENCLOSED BY '' ESCAPED BY '\\' LINES TERMINATED BY '\n' STARTING BY ''`
- In other words, the defaults cause [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") to act as follows when reading input:
	- Look for line boundaries at newlines.
	- Do not skip any line prefix.
	- Break lines into fields at tabs.
	- Do not expect fields to be enclosed within any quoting characters.
	- Interpret characters preceded by the escape character `\` as escape sequences. For example, `\t`, `\n`, and `\\` signify tab, newline, and backslash, respectively. See the discussion of `FIELDS ESCAPED BY` later for the full list of escape sequences.

#### Column List Specification
- By default, when no column list is provided at the end of the [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") statement, input lines are expected to contain a field for each table column
- You must also specify a column list if the order of the fields in the input file differs from the order of the columns in the table
- Each instance of _`col_name_or_user_var`_ in [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") syntax is either a column name or a user variable

#### LOAD DATA INFILE security options
- a security feature that restricts the locations from which files can be read or to which files can be written by specific MySQL operations
- [`secure_file_priv`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_secure_file_priv) used to limit the effect of data import and export operations, such as those performed by the [`LOAD DATA`](https://dev.mysql.com/doc/refman/8.0/en/load-data.html "15.2.9 LOAD DATA Statement") and [`SELECT ... INTO OUTFILE`](https://dev.mysql.com/doc/refman/8.0/en/select-into.html "15.2.13.1 SELECT ... INTO Statement") statements and the [`LOAD_FILE()`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_load-file) function. These operations are permitted only to users who have the [`FILE`](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_file) privilege
- [`secure_file_priv`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_secure_file_priv) may be set as follows:
	- If empty, the variable has no effect. This is not a secure setting.
	- If set to the name of a directory, the server limits import and export operations to work only with files in that directory. The directory must exist; the server does not create it.
	- If set to `NULL`, the server disables import and export operations.
- The server checks the value of [`secure_file_priv`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_secure_file_priv) at startup and writes a warning to the error log if the value is insecure
