### Flask
- numerous libraries in Python for creating web platforms/APIs
- we use Flask
- install with `pip install flask`
- https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world

### What is Flask?
- `Flask` is a microframework
	- it does most of the heavy job of dealing with HTTP requests and responses
- has additional support for automatic JSON serialization / deserialization
- has a development server and can run and test your code on your computer

```python
from flask import Flask  
app = Flask(__name__)  

if __name__ == "__main__"  
	app.run(debug=True)
```
- Use debug=True to set up automatic live reload and informative error pages

### Working with Flask: routes
- to work with your application, use the decorator `@app.route("/something")`
- the return value of the function will be sent at the HTTP response

example
```python
@app.route("/example")  
def example():  
	return "I am running Flask!"
```

### Templates
- we could render the whole HTML page in the return statement but that is impractical
- we use `render_template` method
	- allows to create an HTML template
	- the template can contain variables and basic login and will be interpolated at runtime
- use `render_template('my_template.html', variable1=value1)`
- Flask will look for `my_template.html` in the `templates` folder
- in that template, all references to `{{ variable1 }}` will be replaced by `value1`

#### Logic in templates
- it is possible to insert some logic into replaces instead of views
	- this allows to reuse the same template for different views
- Flask uses Jinja2 for its templates. The documentation has many useful examples

### A simple Flask app with a template

- templates/base.html
```python
<html>
	<body>
	<!-- The value will change depending on the variable sent to the template -->
		Hello, {{ username }}! Here are some numbers from 1 to 5:
		<ul>{% for x in range(1, 6) %}<li>{{ x }}</li>{% endfor %}</ul>
	</body>
</html>
```

- app.py
```python
from flask import Flask, render_template

app = Flask(__name__)
@app.route('/')
def homepage():
	return render_template('base.html', username="John Doe")

if __name__ == "__main__":
app.run()
```

### Use dictionary unpacking to make your `render_template` calls easier to read

```python
render_template("index.html", var1=value1, var2=value2)

# the following is better and easier to manage 
context = {"var1": value1, "var2": value2}  
render_template("index.html", **context)
```

### Routes with arguments
- you may want to define a route that takes an argument 
- for instance `/student/1234` that displays information about student 1234

```python 
@app.route("/student/<int:student_id>")  
def show_student(student_id):  
	return render_template(...)
```
- notice how the function now takes an argument: the value received in the URL

### Flask converters for URL rules


| Item   | Type                                         |
| ------ | -------------------------------------------- |
| string | Default type: accepts any text without slash |
| int    | Positive integers                            |
| float  | Positive integers                            |
| path   | Like strings, but accepts slashes            |
| uuid   |  Accepts UUID strings                        |

### Reversing URLs in templates or Python code

- in python

```python
from flask import url_for 

link = url_for("show student", student_id=1234)
```
- `url_for` takes the name of the function, and all keyword arguments required 

- in a Jinja template

```python
<a href="{{ url_for('show_student', student_id=1234) }}">Student 1234</a>
```

### Template Scaffolding
- as much as you can reuse code in Python, you can create reusable templates
- generally there is a "base" template that other templates can built on 

Example `templates/base.html`
```python
<html>  
<head><title>Example</title></head>  
<body><div class="container">  
{% block main %}  
{% endblock %}  
</div></body>  
</html>
```


### Child template  
- In templates/home.html  

```python 
{% extends "base.html" %}  
{% block main %}  
<h1>Welcome to my Flask website</h1>  
{% endblock %} 
```
 
- In templates/student.html  

```python 
{% extends "base.html" %}  
{% block main %}  
<h1>Student {{ student_id }}</h1>  
{% endblock %}
```

### HTTP error codes with Flask
- in a view, you can also return a tuple
	- the first element is the response 
	- the second element is the status code

```python
@app.route('/data/<value>')  
def show_data(value):  
	if value == "notfound":  
	# We specify the status code in the return value  
		return render_template('error.html'), 404  
	
	else:  
# By default, Flask uses 200 (or 204)  
		return render_template('page.html')
```
