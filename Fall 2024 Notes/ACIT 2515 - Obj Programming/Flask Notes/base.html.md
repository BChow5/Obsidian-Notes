```python
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    </head>
    <body>
        <nav>
            <a href="{{ url_for('home') }}">Home</a>
            <a href="{{ url_for('user') }}">Users</a>
            <a href="{{ url_for('book') }}">Books</a>
            <a href="{{ url_for('category') }}">Categories</a>
            <a href="{{ url_for('available') }}">Available Books</a>
            <a href="{{ url_for('rented') }}">Rented Books</a>
            
        </nav>
        {% block content %}
        {% endblock %}
    </body>
</html>
```

### Book

```python
```python
{% extends "base.html" %}

{% block content %}
<h1>Books</h1>
{% for book in books %}

    <span>
        <p>Title: <a href="{{ url_for('book_detail', book_id=book.id) }}"> {{ book.title }} </a>, Price: {{ book.price }}, Availability: {{ book.available }}, Rating: {{ book.rating }}, UPC: {{ book.upc }}, URL: {{ book.url }}, Category: <a href="{{ url_for('category_detail', name=book.category.name) }}">{{ book.category.name }}</a></p>
    </span>

{% endfor %}
{% endblock %}
```
### Book Detail
```python
{% extends "base.html" %}

{% block content %}
<h1> "{{ book.title }}" Rental Information </h1>

<table>
    <thead>
        <tr>
            <th>User</th>
            <th>Book Title</th>
            <th>Rented Date</th>
            <th>Returned Date</th>
        </tr>
    </thead>
    <tbody>
        {% for rental in books %}
            <tr>
                <!-- Display the user's name -->
                <td>{{ rental.user.name }}</td>
                <!-- Display the associated book title -->
                <td>{{ rental.book.title }}</td>
                <!-- Format the rented date -->
                <td>{{ rental.rented_on.strftime("%Y-%m-%d %H:%M") }}</td>
                <!-- Format the returned date or show 'Not returned' -->
                <td>
                    {% if rental.returned_on %}
                        {{ rental.returned_on.strftime("%Y-%m-%d %H:%M") }}
                    {% else %}
                        Not returned
                    {% endif %}
                </td>
            </tr>
        {% endfor %}
    </tbody>
</table>

{% endblock %}
```

### Category Detail

```python
```python
{% extends "base.html" %}

{% block content %}
<h1>{{ name }} Books</h1>
{% for book in books %}
    <span>
        <p>Title: {{ book.title }}, Price: {{ book.price }}, Availability: {{ book.available }}, Rating: {{ book.rating }}, UPC: {{ book.upc }}, URL: {{ book.url }}, Category: {{ book.category.name }}</p>
    </span>

{% endfor %}
{% endblock %}
```

### User Detail
```python
{% extends "base.html" %}

{% block content %}
<h1>User Details</h1>
<p>User Name: {{ user_name }}</p>
<p>User ID: {{ user_id }}</p>
<h2>Book Rentals</h2>

<table>
    <thead>
        <tr>
            <th>Book Title</th>
            <th>Rented Date</th>
            <th>Returned Date</th>
        </tr>
    </thead>
    <tbody>
        {% for rental in user %}
            <tr>
                <!-- Display the book's title -->
                <td>{{ rental.book.title }}</td>
                <!-- Format the rented date -->
                <td>{{ rental.rented_on.strftime("%Y-%m-%d %H:%M") }}</td>
                <!-- Format the returned date or show 'Not returned' -->
                <td>
                    {% if rental.returned_on %}
                        {{ rental.returned_on.strftime("%Y-%m-%d %H:%M") }}
                    {% else %}
                        Not returned
                    {% endif %}
                </td>
            </tr>
        {% endfor %}
    </tbody>
</table>
<tr>

{% endblock %}
```

