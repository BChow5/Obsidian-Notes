### Return JSON with Flask
- use `jsonify` to automatically return JSON data in your views 

```python
@bp.route("/api/list/")  
def show_list():  
	list_of_dicts = [  
		{  
			"name": "Tim",  
			"city": "Vancouver",  
		},  
		{  
			"name": "John",  
			"city": "Toronto",  
		}  
	]  
	return jsonify(list_of_dicts)
```

### Serialize/deserialize data
- some data types cannot be serialized natively into JSON
- you must serialize them first
- typically using dictionaries (attributes/values)

```python
@bp.route("/api/list/")  
def show_list():  
	manager = ObjectManager()  
	list_of_objects = [  
		obj.to_dict() for obj in manager.get_objects()  
	]  
	return jsonify(list_of_objects)
```

### Process arguments and parameters
```python
@bp.route("/api/detail/<int:number>/")  
def show_detail(number):  
	# Return data for that specific number  
	pass
```

```python
from flask import request  
@bp.route("/api/list/")  
def show_list():  
	order = request.args.get("order", False)  
	# Will match when the request is /api/list?order=yes  
	if order == 'yes':  
		list_of_objects.sort()  
	return jsonify(list_of_objects)
```