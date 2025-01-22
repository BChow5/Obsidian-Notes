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