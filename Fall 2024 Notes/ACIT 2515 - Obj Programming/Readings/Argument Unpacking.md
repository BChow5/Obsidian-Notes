- situations where a function can take a variable number of arguments
	- or where all arguments to a specific funcrion are already available in a dictionary or list
- `*args` (for an interable)
- `**kwargs` for a dicionary

### Positional and Keyword Arguments
- functions take positional and keyword arguments

```python
def func(pos1, pos2, pos3, keyword1="value1", keyword2=None, keyword3=42)  
	pass
```
- `pos1`, `pos2`, `pos3` are positional arguments
	- they are **required** 
	- don't need to provide their names when calling the function
	- e.g. `func (1, "b", None)` is valid
- `keyword1`, `keyword2`, `keyword3` are keyword arguments
	- they are not **required**
	- if not provided, they take default values
- positional arguments always come before keyword arguments
- `*args` is a list of positional arguments
- `**kwargs` is a dictionary of keyword/value arguments/parameters

#### Examples

**Iterable**

```python
def func(*args):  
	print(args)  

func("yes", 2) # prints ["yes", 2]
```

**Dictionary**

```python
def func(**kwargs):
	print(kwargs)

func(example="yes", value=2) # prints {"example": "yes", "value": 2)
```

```python
my_list = ["yes", 2]  
my_dict = {"example": "yes", "value": 2}  

func(*my_list) # equivalent to func("yes", 2)  
func(**my_dict) # equivalent to func(example="yes", value=2)z
```

#### They can also be combined
```python
func("example", *my_list, keyword1="value1", **my_dict)
```

```python
kwargs = {}
if difficulty in ("easy", "medium", "hard"):
	kwargs["difficulty"] = difficulty

if category in get_categories():
	kwargs["category"] = category

if number.isdigit():
	kwargs["number"] = int(number)
	
get_questions(**kwargs)
```