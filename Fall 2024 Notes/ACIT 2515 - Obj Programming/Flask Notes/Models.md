```python
from sqlalchemy import Boolean, Float, Numeric, ForeignKey, Integer, String, DECIMAL, DateTime, select
from sqlalchemy.orm import mapped_column, relationship
from db import db

class Book(db.Model):
    id = mapped_column(Integer, primary_key=True)
    title = mapped_column(String)
    price = mapped_column(DECIMAL(10, 2))
    available = mapped_column(Integer, default=0)
    url = mapped_column(String)
    rating = mapped_column(Integer)
    upc = mapped_column(String)
    category_id = mapped_column(Integer, ForeignKey("category.id"))
    category = relationship("Category", back_populates="books")
    rentals = relationship("BookRental", back_populates="book")

class Category(db.Model):
    id = mapped_column(Integer, primary_key=True)
    name = mapped_column(String)
    books = relationship("Book", back_populates="category")

class User(db.Model):
    __tablename__ = "users"
    id = mapped_column(Integer, primary_key=True)
    name = mapped_column(String)
    rented = relationship("BookRental", back_populates="user")


class BookRental(db.Model):
    __tablename__ = "book_rentals"
    id = mapped_column(Integer, primary_key=True)
    user_id = mapped_column(Integer, ForeignKey("users.id"))
    book_id = mapped_column(Integer, ForeignKey("book.id"))
    rented_on = mapped_column(DateTime(timezone=True), nullable=False)
    returned_on = mapped_column(DateTime(timezone=True), nullable=True)
    user = relationship("User", back_populates="rented")
    book = relationship("Book", back_populates="rentals")
```

### Manage
```python
from sqlalchemy import Boolean, Float, Numeric, ForeignKey, Integer, String, DECIMAL, select, func
from sqlalchemy.orm import mapped_column, relationship
from db import db
from app import app
from datetime import datetime, timedelta
from random import random, randint
from models import Book,Category,User,BookRental
import csv

def create_tables():
    db.create_all()

def delete_tables():
    db.drop_all()

def read_import_csv():
    with open ('data/books.csv', "r", encoding="utf-8") as csv_file:
        reader = csv.DictReader(csv_file)
        for book in reader:
            # Check if the category from the CSV row already exists in the database
            possible_category = db.session.execute(select(Category).where(Category.name == book["category"])).scalar()

            # If the category does not exist, create and add it to the session
            if not possible_category:
                category_obj = Category(name=book["category"])
                db.session.add(category_obj)
            else:
                category_obj = possible_category

            # Update the book's category to be the category object (not just the name)
            book["category"] = category_obj
            book = Book(**book)
            db.session.add(book)
        
    with open ('data/users.csv', "r", encoding="utf-8") as csv_file:
        users = csv.DictReader(csv_file)
        for name in users:
            name = User(**name)
            db.session.add(name)
    
    db.session.commit()

def random_rental(): 

    for n in range(50):
            # Randomly creates a returned on date and time
        now = datetime.now()
        # A book was rented sometime between 10 days ago and 24 days and 4 hours ago
        # Minutes and seconds stay the same
        random_past_offset = timedelta(days=randint(10, 25), hours=randint(0, 5))
        rented_on = now - random_past_offset
        # Decide whether the book was returned (50% chance)
        returned = random() > 0.3
        # If returned, compute a random day (between 2 and 8 days after rental date)
        if not returned:
            returned_on = None
        else:
            returned_on = rented_on + timedelta(days=randint(1, 15), hours=randint(0, 100), minutes=randint(0, 100))
            
        # Assigns random user
        pick_user = select(User).order_by(func.random())
        chosen_user = db.session.execute(pick_user).scalar()

        # Assigns random book
        pick_book = select(Book).order_by(func.random()).where(~Book.rentals.any() | Book.rentals.any(BookRental.returned_on < datetime.now()))
        chosen_book = db.session.execute(pick_book).scalar()

        # Assigns random rental
    
        rental = BookRental(user=chosen_user, book=chosen_book, rented_on=rented_on, returned_on=returned_on)
        db.session.add(rental)
    db.session.commit()

if __name__ == "__main__":
    with app.app_context():
        create_tables() # creates all tables
        read_import_csv()
        random_rental()

    # delete_tables() # drops all tables 
```

### database
```python
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

db = SQLAlchemy(model_class=Base)
```