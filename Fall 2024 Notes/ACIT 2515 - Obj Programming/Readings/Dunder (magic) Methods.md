### Python built-in methods
- everything is an object
- the base type (class) in Python is object
- Python defines several built-in methods (enclosed by `__` as the dunder - double underscore)
- `__init__` is the constructor (initializes values)
- there are other ones
	- `__new__` (the actual "constructor", outside the scope of this course)
	- `__str__` returns a string representation of the object
- the dunder methods are "magic" because the are run without being called explicitly 

#### Example: the `Person` class
```python
class Person:
	def __init__(self, name):
		self._name = name
	def __str__(self)
		return f"Person: (self._name)"
```
- the `__str__` will be called when casting the object into a string (for example, in a `print` statement, or when using `str(my_instance))`.
```python
tim = Person("Tim")

print(tim)
# also possible: str(tim)
```

### Other useful methods to override
- `instance` is an instance of the class considered
- ![[Pasted image 20240930110635.png]]

#### Dunder "mathematical" methids
![[Pasted image 20240930110705.png]]

### Implementing the `__lt__` method for the Score class

```python
class Score:  
	def __init__(self, name, score):  
		self.name = name  
		self.score = score
		  
def __lt__(self, other):  
	if type(other) is not type(self):  
		raise TypeError("Unsupported type")  
	# We compare the score attribute  
	return self.score < other.score
```
- sort the list of scores
- this is making the sort method
```python 
scores = [Score("Tim", 0), Score("John", 1000), Score("Sarah", 2000)]  
print(sorted(scores)) # Will display the sorted result  
scores.sort() # Will sort in place
```

### Design Pattern: Using Collections
- the high scores board is a collection of scores 

```python
class HighScores:  
	def __init__(self):  
		self._scores = list()  
		# We would need to manage scores here, or use aggregation  
	def __len__(self):  
		return len(self._scores)
```

and then:
```python
hiscores = HighScores()  
hiscores.add(tim_score)  
len(hiscores) # Will return 1 (1 score in the list)
```

### Using `operator` for sorting (and other things)
- the standard library comes with the `operator` module
- provides premade functions to access items / elements of an object / collection
- `itemgetter`: to get items from list/dictionaries by key (index for a list or key for a dictionary)
- `attrgetter`: to get attributes from object (by attribute name)
```python
menu = [  
("Pizza", 10),  
("Pizza slice", 3),  
("Fountain drink", 2),  
("Cookie", 4),  
]  

# Each element in the menu is a tuple (~list)  
# We want to sort on the item with index 1  
sorted(menu, key=operator.itemgetter(1))
```
Here's how it works:

- `menu` is a list of tuples, where each tuple contains a menu item (e.g., "Pizza") and its price (e.g., 10).
- `sorted()` is a built-in function that returns a sorted version of the list.
- `key=operator.itemgetter(1)` is used to tell the `sorted()` function to sort by the second element (index 1) of each tuple. In this case, it's the price.

So, the code sorts the `menu` by the prices of the items in ascending order.

The result after sorting:
```python
[("Fountain drink", 2),("Pizza slice", 3),("Cookie", 4),("Pizza", 10)]

```

#### With Dictionaries
```python
menu = [  
{ "name": "pizza", "price": 10, "in stock": 10 },  
{ "name": "drink", "price": 2, "in stock": 50 },  
{ "name": "cookie", "price": 4, "in stock": 20 },  
{ "name": "pizza slice", "price": 3, "in stock": 15 },  
]  
sorted(menu, key=operator.itemgetter('price'))
```

#### with objects
```python
hiscores = [  
Score(name="Tim", score=20),  
Score(name="John", score=0),  
Score(name="Sarah", score=100),  
]  
# Sorting on the name attribute  
sorted(hiscores, key=operator.attrgetter('name'))
```

#### collections: getting elements with `__getitem__`
```python
class HighScores:  
	def __init__(self):  
		# This is a list of scores  
		self._scores = list()
		  
	def __len__(self):  
		return len(self._scores)  

	def __getitem__(self, idx):  
		return self._scores[idx]  

hiscores = HighScores()  
# Add scores to the instance, and then:  
print(hiscores[0].scores) # First score in the list  
print(hiscores[-1].scores) # Last score in the list
```