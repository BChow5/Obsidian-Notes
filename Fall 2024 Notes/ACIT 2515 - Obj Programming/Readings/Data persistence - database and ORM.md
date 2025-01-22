- SQL Alchemy is a module that makes it easy to interact with SQL databases
- install it in your virtual environment with `pip install sqlalchemy`
- Install `Flask-SQLAlchemy` with `pip install flask-sqlalchemy`
- SQL Alchemy has ORM capabilities (Object Relational Mapping) 
- allows to transform SQL rows in a table to python objects and vice versa

### Setting up your engine
```python
from sqlalchemy import create_engine  
engine = create_engine("sqlite:///bing.db", echo=True)
```
- Set echo to False to stop seeing the translated SQL queries in the terminal

### Setting up your Session Class
```python
# File: database.py  
from sqlalchemy.orm import sessionmaker  
Session = sessionmaker(bind=engine)
```

### Always use a session
```python
from database import Session

# Option 1
session = Session()
# do stuff

# Option 2
with Session() as session:
	# do stuff
```

### Create your Base class
```python
from sqlalchemy.orm import DeclarativeBase  

class Base(DeclarativeBase):  
	pass
```

### Create a database-backed class
```python
from sqlalchemy import String, DECIMAL, Integer, ForeignKey  

class Book(Base):  
	__tablename__ = "books"  

	id = mapped_column(Integer, primary_key=True)  
	title = mapped_column(String)  
	price = mapped_column(DECIMAL(10, 2))  
	available = mapped_column(Integer, default=0)
```
- you always need a primary key
- column types are defined by SQL alchemy 

### Create the tables

`Base.metadata.create_all()`

### Use sessions to make queries (execute statements)
- create a statement (usually with `select`)
- execute the statement (`session.execute)`
- get the results (`.scalars()` or `.scalar()`)
	- scalars if we're expecting multiple things from the result 

```python
from sqlalchemy import select

statement = select(Book)  
results = session.execute(statement)  
books = results.scalars()
```

#### Use the session to add rows
```python
# Remove book with ID = 10  
book = session.execute(select(Book).where(Book.id == 10)).scalar()  
session.delete(book)  
session.commit()
```

### Statements
- statements are SQL-like python objects
- some useful operations include:
- **where clauses (can be combined)**  
	- `select(Book).where(Book.rating <= 1)`  
	- `select(Book).where(Book.rating >= 3).where(Book.title.ilike("%last%"))  `
- **ordering**  
	- `select(Book).where(Book.available > 0).order_by(Book.rating)  `
- **reverse ordering**  
	- `select(Book).order_by(Book.rating.desc()) (SQL descending)`

### Functions
- backend specific functions can be useful (like RANDOM for example)
```python
from sqlalchemy.sql import func  
# Random sort using SQL RANDOM function  
select(Book).where(Book.available > 0).order_by(func.random())
```

### Using results
- After executing your statement, you can extract the results. Use the following methods:  
	- `.scalars()` : returns a list of result objects  
	- `.scalar()` : returns one result only  
	- `.first()` : returns the first object or None

### Be careful with AI help
- usually outdated for SQL alchemy
- Do NOT use query !

### Relationships/foreign keys: one to many
- you must define a `ForeignKey` column
- the `ForeignKey` links to the `primary_key` of the other table
- use `relationship` fields to automatically fetch the related objects
- make sure you understand how `back_populates` works

#### back_populates
- `back_populates` is used to define a bidirectional relationship between two mapped classes
- `back_populates` ensures that changes to one side of the relationship automatically reflect on the other side.
- **Both Sides of the Relationship Must Use `back_populates`**: For `back_populates` to work, you must define it on both sides of the relationship. If you miss one side, the relationship will not be bidirectional.

#### One to many python code
```python
class Book(Base):  
	# [...] other fields  
	category_id = mapped_column(Integer, ForeignKey("categories.id"))  
	category = relationship("Category", back_populates="books")  

class Category(Base):  
	__tablename__ = "categories"  
	
	id = mapped_column(Integer, primary_key=True)  
	name = mapped_column(String)  
	books = relationship("Book", back_populates="category")
```

#### One to many: using relationship attributes
```python
book = session.execute(select(Book).order_by("?")).scalar()  
# Relationship from Book to Category (one)  
print(book.category.name)  

poetry = session.execute(select(Category).where(name="Poetry")).scalar() 
# Relationship from Category to Book(s) (many)  
for book in poetry.books:  
print(book.title)
```

### Many to many: association object
- use an association object, with two foreign keys to each class you want to link
- carefully name and use the relationship fields to set up associations
- like associative tables in SQL  

#### Rental Books as the Library
```python
class User(Base):  
	__tablename__ = "users"  
	
	id = mapped_column(Integer, primary_key=True)  
	name = mapped_column(String)  
	rentals = relationship("BookRental", back_populates="user")

class BookRental(Base):  
	__tablename__ = "book_rentals"  
	
	id = mapped_column(Integer, primary_key=True)  
	user_id = mapped_column(Integer, ForeignKey("users.id"))  
	book_id = mapped_column(Integer, ForeignKey("books.id"))  
	rented_on = mapped_column(DateTime(timezone=True), nullable=False)  
	returned_on = mapped_column(DateTime(timezone=True), nullable=True)  
	user = relationship("User", back_populates="rentals")  
	book = relationship("Book", back_populates="rentals")
```

#### Rent Books
- create `BookRental` instances
```python
from datetime import datetime, timedelta  
tim = session.execute(select(User).where(name == "Tim")).scalar()  
book = session.execute(select(Book).where(title == "ACIT2515")).scalar()  
another_book = session.execute(select(Book).where(Book.id == 10)).scalar()  

rental1 = BookRental(user=tim, book=book, rented_on=datetime.now()) 

yesterday = datetime.now() - timedelta(days=1)  
rental2 = BookRental(user=time, book=another_book, rented_on=yesterday)  

session.add(rental1)  
session.add(rental2)  

session.commit()
```

#### Find unreturned books for Tim
```python
stmt = select(BookRental).where(BookRental.user == tim).where(BookRental.returned_on != None)  
books_not_returned = [result.book.title for result in session.execute(stmt).scalars()]
```

or if you want to do it more python way

```python
ooks_not_returned = []
for rental in tim.rentals:
	if rental.returned is None:
		books_not_returned.append(rental.book)
		
books_not_returned = [rental for rental in tim.rentals if rental.returned_on is None]

```

#### Find all the "Travel" books Tim ever rented
```python
travel = session.execute(select(Category).where(Category.name == "Travel")).scalar()  
stmt = select(BookRental).where(BookRental.user == tim).where(BookRental.book.has(category=travel))  
results = session.execute(stmt).scalars() 

for association in results:  
	print(association.book.title)
```

#### Find all the people who rented a specific book with the dates
```python
book = session.execute(select(Book).where(Book.title == "ACIT2515")).scalar()  
for rental in book.rentals:  
	print(rental.user.name, rental.rented_on, rental.returned_on)
```