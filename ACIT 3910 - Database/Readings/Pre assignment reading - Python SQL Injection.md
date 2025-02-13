```
select admin from users where username = ''; select true; --'
```

`select admin from users where username = '';`
This is your intended query. The semicolon (`;`) terminates the query, so the result of this query does not matter. Next up is the second statement:

`select true;`

This statement was constructed by the intruder. It’s designed to always return `True`.

Lastly, you see this short bit of code:

`--'`

This snippet defuses anything that comes after it. The intruder added the comment symbol (`--`) to turn everything you might have put after the last placeholder into a comment.

### Crafting Safe Query Parameters
- To make sure values are used as they’re intended, you need to **escape** the value
- modern database adapters, come with built-in tools for preventing Python SQL injection by using **query parameters**
- Different adapters, databases, and programming languages refer to query parameters by different names. Common names include **bind variables**, **replacement variables**, and **substitution variables**

```python
def is_admin(username: str) -> bool:
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT
                admin
            FROM
                users
            WHERE
                username = %(username)s
        """, {
            'username': username
        })
        result = cursor.fetchone()

    if result is None:
        # User does not exist
        return False

    admin, = result
    return admin
```

Here’s what’s different in this example:

- **In line 9,** you used a named parameter `username` to indicate where the username should go. Notice how the parameter `username` is no longer surrounded by single quotation marks.
    
- **In line 11,** you passed the value of `username` as the second argument to `cursor.execute()`. The connection will use the type and value of `username` when executing the query in the database.

### Passing Safe Query Parameters
- **Named placeholders** are usually the best for readability, but some implementations might benefit from using other options

```python
# SAFE EXAMPLES. DO THIS!
cursor.execute("SELECT admin FROM users WHERE username = %s'", (username, ));
cursor.execute("SELECT admin FROM users WHERE username = %(username)s", {'username': username});
```
- In these statements, `username` is passed as a named parameter. Now, the database will use the specified type and value of `username` when executing the query, offering protection from Python SQL injection.

### Using SQL Composition
- **Literals** are values such as numbers, strings, and dates
- As you’ve seen already, the database adapter treats the variable as a string or a literal. A table name, however, is not a plain string

```python
from psycopg2 import sql

def count_rows(table_name: str) -> int:
    with connection.cursor() as cursor:
        stmt = sql.SQL("""
            SELECT
                count(*)
            FROM
                {table_name}
        """).format(
            table_name = sql.Identifier(table_name),
        )
        cursor.execute(stmt)
        result = cursor.fetchone()

    rowcount, = result
    return rowcount
```
- There are two differences in this implementation. 
	- First, you used `sql.SQL()` to compose the query. 
	- Then, you used `sql.Identifier()` to annotate the argument value `table_name`. (An **identifier** is a column or table name.)

```python
from psycopg2 import sql

def count_rows(table_name: str, limit: int) -> int:
    with connection.cursor() as cursor:
        stmt = sql.SQL("""
            SELECT
                COUNT(*)
            FROM (
                SELECT
                    1
                FROM
                    {table_name}
                LIMIT
                    {limit}
            ) AS limit_query
        """).format(
            table_name = sql.Identifier(table_name),
            limit = sql.Literal(limit),
        )
        cursor.execute(stmt)
        result = cursor.fetchone()

    rowcount, = result
    return rowcount
```
- In this code block, you annotated `limit` using `sql.Literal()`. As in the previous example, `psycopg` will bind all query parameters as literals when using the simple approach. However, when using `sql.SQL()`, you need to explicitly annotate each parameter using either `sql.Identifier()` or `sql.Literal()`.

![[Pasted image 20250131205341.png]]