### URLs
- URLs have the following format: `protocol://domain/whatever/path?value1=10&value2=abc`
	- protocol is the protocol typically `http` or `https`
	- domain is the domain name
		- e.g. `learn.bcit.ca`
	- `/whatever/path` is the resource path
		- this is what we match in a flask decorator 
	- `?value1=10&value2=abc` are URL parameters, also known as a query string
		- contains additional arguments when accessing a resource path

#### Accessing the query string in Flask

```python
# You need to import `request` to access the request parameters
from flask import Flask, request
# [...]
@app.route("/")
def home():
	value1 = request.args.get("value1")
	# You can also set more options
	value1 = request.args.get("value1", default=1000)
	# The following will cast value2 into an integer automatically - no type
conversion required
	value2 = request.args.get("value2", default=1000, type=int)
	return "See console"
```
- query strings are useful to adjust the way content is displayed
- they should **absolutely never** be used to carry any sensitive information
- they are high insecure by design
- URLs have a maximum length
	- query strings are part of the URL, so there is a limit to the information you can carry in them
- a typical use case would be:
	- specifying the sorting order (sort by title, sort by date): `/books?sort=title`, `/rentals?sort=date`
	- specifying a list of categories you're looking for

### JSON, HTTP methods and payloads
- due to limitations of query string, we can send data to the server from the client
- an HTTP request can have its own body
	- this is what happens when you submit a form in an HTML page
- the "body" can be encoded in various ways
	- common to use JSON
	- JSON is understood by almost any programming language and is a serialization format of choice for APIs 
- usually only makes sense to have a body when the request is supposed to change something on the server (the body contains info about how the server data should change)

#### `POST`  vs `PUT` (and `DELETE`)
- many API developers and frameworks use `POST` regardless of the need
- you should use `POST` to **create** a resource
- use `PUT` to **create or update** a resource (knowing it's identifier)
- `DELETE` used to delete a resource 
- `GET` is used to request data or resources from a server
	- `GET` is the default method for a route
	- used to retrieve or display info without modifying the server's state
		- e.g. loading webpage, fetching search result, requesting an image or file

| URL      | Method | Expected action on the server                                                        |
| -------- | ------ | ------------------------------------------------------------------------------------ |
| /books   | POST   | create a new book                                                                    |
| /books   | PUT    | should be **invalid**. Cannot use PUT without identifying the resource it applies to |
| /books/1 | POST   | **invalid**. cannot use POST with an existing resource                               |
| /books/1 | PUT    | creates a book with ID 1, or update the existing book with ID 1                      |
| /book/1  | DELETE | Deletes the book with ID 1                                                           |
| /books   | DELETE | **invalid**. cannot use DELETE without identifying the resource it applies to        |

#### Matching the HTTP method and accessing the request body in Flask

```python
from flask import Flask, request
@app.route("/books", methods=["POST"])
def create_book():
	data = request.get_json()
	print(data)
 
@app.route("/books/<int:book_id>", methods=["PUT"])
def update_book(book_id):
	data = request.get_json()
	print(data)
```
- you can use `request.get_json()` to automatically decode the request body into JSON
- use `methods=[]` to list the HTTP methods accepted by the route
- Be careful! This only works when the HTTP request MIME type is set to application/json. Otherwise, it returns None!

#### Returning JSON in Flask
- return a type that is supposed by JSON (a list or dictionary)
- all elements in the collection must be compatible with JSON
	- e.g. a list of objects will not work because objects are not serializable to JSON by default
- use `jsonify` to convert compatible data to JSON
	- will need to convert objects to dictionaries first 

```python
from flask import Flask, jsonify

@app.route("/") 
def home(): 
	return jsonify({"a": [1, 23, 56], "b": "hello"})
```