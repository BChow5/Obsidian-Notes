```python
from flask import Flask, render_template
import csv
from pathlib import Path
from db import db
from sqlalchemy import select
from models import Book, Category, User, BookRental
from datetime import datetime

app = Flask(__name__)

# This will make Flask use a 'sqlite' database with the filename provided
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///i_copy_pasted_this.db"
# This will make Flask store the database file in the path provided
app.instance_path = Path("./data").resolve()
# Adjust to your needs / liking. Most likely, you want to use "." for your instance path.
# You may also use "data" - it will be the name of the folder where your database is stored.

# Initialize the database with the Flask app
db.init_app(app)

# Route for the home page
@app.route("/")
def home():
    # Renders the "home.html" template when the home page is accessed
    return render_template("home.html")

# Route for the users listing page
@app.route("/users")
def user():
    # Select all users from the database
    user_stmt = select(User)
    # Execute the query and retrieve the list of users
    user_list = db.session.execute(user_stmt).scalars()
    # Render the "users.html" template with the list of users
    return render_template("users.html", users=user_list)

# Route for displaying a specific user's details
@app.route("/users/<int:user_id>")
def user_detail(user_id):
    # Query for the rental records associated with the specific user
    user_page_stmt = select(BookRental).where(BookRental.user_id == user_id)
    user_page = db.session.execute(user_page_stmt).scalars()

    # Query for the name of the user based on the user_id
    user_name = db.session.execute(select(User.name).where(User.id == user_id)).scalar()

    # Render the "user_detail.html" template with the rental records and user details
    return render_template("user_detail.html", user=user_page, user_id=user_id, user_name=user_name)

# Route for the books listing page
@app.route("/books")
def book():
    # Select all books from the database
    book_stmt = select(Book)
    # Execute the query and retrieve the list of books
    book_list = db.session.execute(book_stmt).scalars()
    # Render the "books.html" template with the list of books
    return render_template("books.html", books=book_list)

# Route for displaying details of a specific book
@app.route("/books/<int:book_id>")
def book_detail(book_id):
    # Query for rental records associated with the given book ID
    book_page_stmt = select(BookRental).where(BookRental.book.has(Book.id == book_id))
    book_page = db.session.execute(book_page_stmt).scalars()

    # Query for the book information based on the book ID
    book_info_stmt = select(Book).where(Book.id == book_id)
    book_info = db.session.execute(book_info_stmt).scalar()  # Use scalar() to get a single record

    # Render the "books_page.html" template with the rental records and book ID
    return render_template("books_page.html", books=book_page, book=book_info)

# Route for the categories listing page
@app.route("/categories")
def category():
    # Select all categories from the database
    category_stmt = select(Category)
    # Execute the query and retrieve the list of categories
    category_list = db.session.execute(category_stmt).scalars()
    # Render the "categories.html" template with the list of categories
    return render_template("categories.html", categories=category_list)

# Route for displaying books in a specific category
@app.route("/categories/<string:name>")
def category_detail(name):
    # Query for books that belong to the given category name
    stmt = select(Book).where(Book.category.has(Category.name == name))
    category = db.session.execute(stmt).scalars()
    # Render the "categories_detail.html" template with the books in that category
    return render_template("categories_detail.html", books=category, name=name)

# Route for displaying available books (books with no rentals or with a returned rental)
@app.route("/available")
def available():
    # Query for books that are either not rented or have a rental that has been returned
    pick_book = select(Book).where(~Book.rentals.any() | Book.rentals.any(BookRental.returned_on < datetime.now()))
    available_books = db.session.execute(pick_book).scalars()
    # Render the "available.html" template with the available books
    return render_template("available.html", books=available_books)

# Route for displaying books that are currently rented
@app.route("/rented")
def rented():
    # Query for books that are rented and have not been returned
    pick_book = select(Book).join(Book.rentals).where(
        BookRental.rented_on < datetime.now(),  # Rental date is before the current time
        BookRental.returned_on == None  # Rental does not have a return date (still borrowed)
    )
    borrowed_books = db.session.execute(pick_book).scalars()
    # Render the "rented.html" template with the list of rented books
    return render_template("rented.html", books=borrowed_books)




if __name__ == "__main__":
    app.run(debug=True, port=8888)

```