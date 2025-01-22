### appy.py
```python
from flask import Flask, render_template, jsonify, request
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

    # Query for the book information based on the book ID
    book_info_stmt = select(Book).where(Book.id == book_id)
    book_info = db.session.execute(book_info_stmt).scalar()  # Use scalar() to get a single record

    # Render the "books_page.html" template with the rental records and book ID
    return render_template("books_page.html", book=book_info)

@app.route("/api/books")
def api_book():
    # Select all books from the database
    book_stmt = select(Book)
    # Execute the query and retrieve the list of books
    book_list = db.session.execute(book_stmt).scalars()
    # Render the "books.html" template with the list of books
    # Initialize an empty list to store dictionaries
    book_dictionary = []

    # Convert each book to a dictionary
    for book in book_list:
        book_temp = book.to_dict()  # Call the to_dict method on each book
        book_dictionary.append(book_temp)  # Add the dictionary to the list
    book_json = jsonify(book_dictionary)
    return book_json

@app.route("/api/books/<int:book_id>")
def api_book_detail(book_id):
    # Query the database for the book with the given ID
    book = db.session.get(Book, book_id)
    
    # If the book does not exist, return a 404 error
    if not book:
        return {"error": "Book not found"}, 404
    
    is_available = True
    for rental in book.rentals:
        if rental.returned_on is None:
            is_available = False
            break

    # Add the `available` attribute to the book dictionary
    book_dict = book.to_dict()
    book_dict["available"] = is_available

    # Return the book data as JSON
    return jsonify(book_dict)

@app.route("/api/books", methods=["POST"])
def create_book():
    # parse incoming data sent by POST for JSON data
    data = request.get_json()

    # set up for validating the data
    is_positive_number = lambda num: isinstance(num, (int, float)) and num >= 0
    is_non_empty_string = lambda s: isinstance(s, str) and len(s) > 0
    is_valid_rating = lambda num: isinstance(num, int) and 1 <= num <= 5
    # Field mapping
    required_fields = {
    "title": is_non_empty_string,
    "price": is_positive_number,
    "available": is_positive_number,
    "rating": is_valid_rating,
    "url": is_non_empty_string,
    "upc": is_non_empty_string,
    "category": is_non_empty_string,
    }

    # Loop through each field in the required_fields dictionary
    for field in required_fields:
        # Check if the field is missing from the request data
        if field not in data:
            # If the field is missing, return an error message with HTTP status 400 (Bad Request)
            return {"error": f"Missing field: {field}"}, 400
        else:
            # If the field is present, retrieve its associated validation function from the dictionary
            test_func = required_fields[field]
            # Extract the value of the current field from the request data
            value = data[field]
            # Validate the value using the validation function
            if not test_func(value):
                # If the validation fails, return an error message with HTTP status 400
                return {"error": f"Invalid value for field {field}: {value}"}, 400
    
    # Check if the book with the same UPC already exists in the database
    existing_book = db.session.execute(select(Book).where(Book.upc == data["upc"])).scalar()
    if existing_book:
        return {"error": "A book with this UPC already exists."}, 400

    # Check if the category already exists in the database
    possible_category = db.session.execute(select(Category).where(Category.name == data["category"])).scalar()

    # If the category does not exist, create and add it to the session
    if not possible_category:
        category_obj = Category(name=data["category"])
        db.session.add(category_obj)
        db.session.commit()  # Commit here to persist the category before using it
    else:
        category_obj = possible_category  # Use the existing category

    # Create a new Book instance with the validated data
    new_book = Book(
        title=data["title"],
        price=data["price"],
        available=data["available"],
        rating=data["rating"],
        url=data["url"],
        upc=data["upc"],
        category=category_obj
    )

    # Add the new book to the session and commit the transaction
    db.session.add(new_book)
    db.session.commit()

    book_dict = new_book.to_dict()
    return jsonify(book_dict), 201

@app.route("/api/books/<int:book_id>/rent", methods=["POST"])
def book_rental(book_id):
    # parse incoming data sent by POST for JSON data
    data = request.get_json()

    # Validate if user_id is present in the data
    if "user_id" not in data:
        return {"error": "Missing field: user_id"}, 400
    user_id = data["user_id"]

    # Fetch the book from the database using its primary key
    book = db.session.get(Book, book_id)
    # Check if the book exists, if not return 404
    if not book:
        return {"error": "Book not found"}, 404
    
    # Check if the book is available (not rented out)
    is_available = True
    for rental in book.rentals:
        if rental.returned_on is None:  # If there is an active rental
            return {"error": "Book is not available"}, 403
        
    # Check if the user exists in the database
    user = db.session.get(User, user_id)
    if not user:
        return {"error": "User not found"}, 404

    # Create a new rental for the book
    add_rental = BookRental(
        user_id=user_id,
        book_id=book_id,
        rented_on=datetime.now()
    )

    # Add the new rental to the session and commit the transaction
    db.session.add(add_rental)
    db.session.commit()

    # Return a success message along with the rental details
    return jsonify({
        "rented": "Book successfully rented",
        "book_id": book_id,
        "rental_id": add_rental.id,
        "rented_on": add_rental.rented_on,
        "user_id": user_id
    }), 200

@app.route("/api/books/<int:book_id>/return", methods=["PUT"])
def book_return(book_id): 
    # Fetch the active rental record for the given book
    rental = db.session.execute(select(BookRental).where(BookRental.book_id == book_id, BookRental.returned_on == None)).scalar()

    # Check if no active rental exists
    if rental == None:
        return {"error": "The book was not rented"}, 403

    # Mark the book as returned by updating the returned_on field
    rental.returned_on = datetime.now()
    db.session.commit()

    return {"message": "Book has been returned."}, 200
        
    
        

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
    pick_book = select(Book).where(~Book.rentals.any() | ~Book.rentals.any(BookRental.returned_on == None))
    available_books = db.session.execute(pick_book).scalars()
    # Render the "available.html" template with the available books
    return render_template("available.html", books=available_books)

# Route for displaying books that are currently rented
@app.route("/rented")
def rented():
    # Query for books that are rented and have not been returned
    pick_book = select(Book).where(Book.rentals.any(BookRental.returned_on == None))    
    borrowed_books = db.session.execute(pick_book).scalars()
    # Render the "rented.html" template with the list of rented books
    return render_template("rented.html", books=borrowed_books)




if __name__ == "__main__":
    app.run(debug=True, port=8888)

```