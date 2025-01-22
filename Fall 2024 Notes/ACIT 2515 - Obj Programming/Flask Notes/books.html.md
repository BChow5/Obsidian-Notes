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