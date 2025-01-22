### General
- EVERYTHING in python is an object
- the base type (class) in Python is an object
- **generator** is a special type of iterator that allows you to iterate over a sequence of values without storing the entire sequence in memory at once
	- class generator
- PEP8 Naming: def get_user_info():
- Open file to write: with open("file.txt", "w") as fp:

### Variables
- declare variable - `my_variable = something`
- variables are locally scoped inside a function
- variables follow snakecase - `my_great_variable`

### Numbers and arithmetic 
- types: `int`, `float`, `complex`
- div_result = a / b  (float division) 
- floor_div_result = a // b (integer division)
- mod_result = a % b (mod/remainder)
- exp_result = a ** b (exponent)

### Booleans
- `==` to check if two values are equal. `!=` for not equal
- booleans are only `True` or `False`
- combine logic with `and`, `or`, `not`
- `None` is also a built in value (neither true or false)

### Collections
- `lists` - `[1,2,3]`
- `tuple` - `(1,2,3)`
- `set` - `{1,2,3}`
- `dict` - `dict_a = {'a': 1, 'b': 2}`
- lists and tuples are ordered (sortable)
- lists and sets are mutable 
- sets have no duplicates 

#### Lists and Tuples
- `sequence[index]` to access a specific item in list
- `.append()` - add single item at the end of a list
- `.extent()` - add all elements of an iterable (like list or set) to end of a list
- `.insert()` - insert element at specific position in list
	- my_list.insert(1, 4) # Now my_list is `[1, 4, 2, 3]`
- `.copy()` - returns a copy of a list. make a new list with the same elements of the original list
- `.pop()` - remove and return the element at the specified position in the list. if no specify, then it removes last item
- `.reverse()` reverses the elements of the list in place
- Tuples in Python are **immutable**, meaning that once they are created, their elements cannot be changed or modified directly
```python
# Lists  
my_list = [4, 5, 6]  
another_list = list(1, 2, 3) # Another way to define a list  
my_list.append("something") # Add an element at the end of the list  
my_list[0] # Access an element by its position in the list  
(starts at 0)  
my_list[2] = "hello" # Change values  
new_list = my_list + another_list # Concatenate lists with +  
my_list.extend(another_list) # Similar to above, but modifies in place!  

# Tuples  
my_tuple = (1, 2, 3)  
another_tuple = tuple("hello", "world")  
my_tuple[1] # like lists  
my_tuple[0] = 0 # ERROR! Tuples are immutable
```

```python
def my_function(arg1, arg2, arg3="quiz"):
  return "hello", "goodbye" # returns as one tuple
```
#### Useful functions for lists 
```python
my_list = [1, 2, 3, 4, "hello"]  
len(my_list) # Number of elements in the list  
5 in my_list # False: 5 is not in the list  
"hello" in my_list # True
```
#### Sets
```python
my_set = set(1, 1, 1, 2, 3) # Elements are unique - the set contains {1, 2, 3}  
another_set = {"hello", "hello"}  
s[0] # ERROR! Sets are not ordered or indexed
```
- Adding to set: my_var.add(element)

#### Dictionaries
- type `dict`
- maps keys to values
- dictionaries are not ordered. can't sort
- `dictionary[my_key]` to access element `my_key` in `dictionary`
- `.keys()` returns a list of all the keys in the dictionary
- `.values()` returns a list of all the values in the dictionary
- `.items()` returns a list of tuples, each containing a key-value pair
- `.update()` add a key-value pair

```python
my_dict = {"hello": "goodbye", 1234: "sunshine"}  
my_dict = dict((("hello", "goodbye"), (1234, "sunshine")))  
my_dict["hello"] # 'goodbye'  
my_dict["test"] = "something" # Add values to the dict  
my_dict[1234] = "rain" # Keys are unique  
my_dict.keys() # List of keys  
my_dict.values() # List of values  
my_dict.items() # List of key/value pairs
```

```python 
for key, value in my_dict.items():
print("Key:", key, " - Value:", value)

# Similar, by using keys
for key in my_dict.keys():
	print("Key:", key, " - Value:", my_dict[key])

# Not using keys, looping on values only
for value in my_dict.values():
print("Value:", value)
```

### Strings (text)
- string are immutable
- type `str`
- `.split()`,`.join()`,`.upper()`,`.loewr()`, `isnumeric()`
- `"abc de f".split() => ["abc", "de", "f"]`  
- `" ".join(["abc", "de", "f"]) => "abc de f"`

```python
my_string = "hello world"  
my_string[0] # 'h'  
my_string[0] = "H" # ERROR! Strings are immutable  
my_string.upper() # 'HELLO WORLD'  
my_string.split(' ') # ['hello', 'world']  
' '.join(['hello', 'world']) # 'hello world'  
'HELLO WORLD'.lower() # 'hello world'  
'world' in my_string # True

for letter in my_string:  
	print(letter) # to iterate
```


### Loops
- **`break`**: Exits the loop completely.
- **`continue`**: Skips the rest of the current iteration and moves on to the next iteration.
#### For Loops
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

for i in range(5):
    print(i)

for char in "hello":
    print(char)
```

#### For Loop - dictionary
```python
my_dict = {'a': 1, 'b': 2, 'c': 3}

# Loop through keys
for key in my_dict:
    print(key)

# Loop through values
for value in my_dict.values():
    print(value)

# Loop through key-value pairs
for key, value in my_dict.items():
    print(f"{key}: {value}")
```

#### While Loop
```python
i = 0
while i < 5:
    print(i)
    i += 1
```

### Functions
- functions can receive parameters/arguments 
```python
def register_student(student_id, name, international=False, scholarship=False):  
# Do something with the parameters  
return True
```
- keyword arguments can be provided in any order if you use their names
- Making a Function: def greet_user(): 

### Return
- all functions return something with `return`
- if `return` is missing, then function returns `None`
- a function returns ONE value but the value can be a collection (list, set, tuple, dictionary, etc.)

### Exceptions
- errors in code raise exception
- catch exceptions using `try...except`
```python
def say_hello(name):  
	if name == "Tim":  
		raise RuntimeError("This name is not allowed.")  
	return f"Hello {name}."
```

```python
try:  
	text = say_hello("Tim")  
	print(text)  
except RuntimeError:  
	print("The name you provided is not allowed")
finally: 
	file.close() # This will run whether or not an exception occurs
```

```python
try:
    value = int(input("Enter a number: "))
except (ValueError, ZeroDivisionError):
    print("An error occurred: invalid input or division by zero")
```

#### Common Errors
- **ValueError**: Raised when a function receives an argument of the correct type but inappropriate value.
- **TypeError**: Raised when an operation or function is applied to an object of an inappropriate type.
- **IndexError**: Raised when trying to access an index that is out of range for a list or other sequence.
- **AttributeError**: Raised when an invalid attribute reference is made.
- **RuntimeError**: catch-all for errors that occur during runtime and are not handled by other more specific exceptions

### Files and Paths
```python
with open("my_file.txt", "r") as fp:  
	data = fp.read()
```
- you should use relative paths
	- e.g. `data/file.txt`

### Imports and Packages
- you can `import` other files and/or `import` only specific symbols of a module
```python
# Contents of main.py  
import my_print  

def main():  
	my_print.my_print_func("Example.")  
	print(my_print.MY_MESSAGE)
```
- you can access "properties" of the module like you access "behaviours" for data types with `.`
- use `if __name__ == "__main"`
- packages are a collection of modules 

importing subpackes
```python
import logic.game  
logic.game.start() # OK  
logic.computer.aimbot.shoot() # DOES NOT WORK (we only have logic.game)
```
- When Python encounters an import statement and the symbol imported is a package, the __init__.py file will automatically be run

### Unit Tests and Pytest
- can be automated and scheduled. they are also fast to run
-  testing frameworks like `unittest` and `pytest`
	- `pip install pytest`
- test files are any with titles like
	- `test_*.py` or `*_test.py`
- test functions must start with `test_`
- Use “assert” to state the right value of test 
- test can be functions outside of classes or methods inside classes
```python
# test_something.py  
def test_thingy(): # This is run by pytest  
	pass  

def thing(): # But not this  
	pass  

class Something:  
	def test_thing(self): # Also not this!  
		pass  

class TestSomething:  
	def test_thing2(self): # This is run by pytest  
		pass
```

```python
def test_add_values():  
	result = add_values(2, 3)  
	assert result == 5 # This line makes sure the output is the one we expect
```

```python
def add_values(a, b):  
	if type(a) is not int or type(b) is not int:  
		raise TypeError("Invalid value")  
	return a+b
```

#### Code Coverage and Unit Testing
- aim for at least 80% code coverage 
- `pip install pytest-cov`
- add coverage with the `--cov` option when you run your test

### Object Oriented Programming
- **Class** - defines a general category (e.g. book, bank account)
	- blueprint (or template) for creating an object
- **Object or instance** - a thing created out of a class
- **Attributes** - values for a specific object
- **Methods** - behaviours (or capabilities) of a class
- **State** - the current values of the attribute in an object

#### Concepts
- **Modelization**
	- objective of a model is to "describe" an object
	- We represent the objects by creating a model, and implement it using a class
- **Encapsulation** (and visibility)
	- state of an object should only be changed by the object itself
	- Using private attributes and using methods to change the state is called encapsulation
- **Abstraction**
	- implementation of the behaviours of the object is the responsibility of the class 
	- other objects should not depend upon private attributes or methods
	- they should only use public interface of the class - the public methods and attributes 

### Class Syntax
- `class MyClassName` to define a new class "block"
- classes should be named with `PascalCase`
- except for class variables, a class only has methods
- methods are defined in the class block and receive `self` as first parameter
- special method `__init__` is auto called upon initialization 
	- also known as the constructor 

```python
class Student:  
	def __init__(self, name, student_number):  
		self.name = name  
		self.student_number = student_number 
		self.program = "CIT"  
		
def display(self):  
	print(f"{self.name}, {self.student_number} - {self.program})
	  
john = Student("John Doe", "A01234567")  
john.display()  
bob = Student("Bobby", "A4432890")  
bob.display()
```

```python
class Student:
	def __init__(self, name, student_number):
		if not name or type(name) is not str:
			raise AttributeError("Name cannot be empty!")
		self.name = name
		self.student_number = student_number
		self.program_name = "CIT"

john = Student("", "A01234567") # Will raise an Exception!
john = Student(42, "A01234567") # Will raise an Exception!
```

### `self` parameter
- all functions defined in a class are called "methods"
- `self` parameter represents the current instance 
- each instance has its own version of the class attributes and they are accessible with `self` (encapsulation)
- NEVER use `self` outside the class block

### Python `property`
- allows you to define methods that act like attributes of a class, giving you more control over how you get, set, or delete an attribute's value
- `@property` 
- allows you to define **getter**, **setter**, and **deleter** methods for attributes

```python
class Student:  
[...]  
@property  
def program(self):  
	return self.program_name 

@program.setter  
def program(self, value):  
	if value not in ("CIT", "CST", "BTech"):  
		raise ValueError("Invalid program name!")  

	self.program_name = value  

john = Student("John Doe", "A01234567")  
print(john.program) # Runs the getter method (@property)  
john.program = "Btech" # Runs the setter method (@property.setter)  
john.program = "something" # Raises an Exception! (in @property.setter)
```

### Static Methods and Class Methods
- can define methods that are common to all objects of the same class
	- not tied to individual instances of class
- static methods do not get any reference to the class or object
	- a method that doesn’t require access to either the instance (`self`)
- class methods receive a reference to the class when called directly from the class name called `cls`
- `@staticmethod` decorator for static methods (no reference)
- `@classmethod` decorator for class methods (reference to class)
- **Use `@classmethod`** when you need to modify or access class-level attributes, or when you want to create alternative constructors for the class.
- **Use `@staticmethod`** for utility methods that don't need access to class or instance data but logically belong to the class.

```python
class Student:  
[...]  
@staticmethod  
def check_program(value):  
# This is a static method. It does not receive an implicit argument.  
# It does not have access or know about the class / instance  
	return value in ("CIT", "CST", "BTech")  

Student.check_program("CIT") # will return True  

john = Student("John Doe", "A01234567")  
john.check_program("CIT") # will also return True
```

### Class Variables
- attributes common to all objects of the same class
- defined in class block but outside any methods

### Object Relationships
- you can create aggregation/composition relationship by keeping a "reference" from one object to another 

e.g. composition
```python
class Finger:  
	def flick(self):  
		print("Oh no.")  

class Hand:  
	def __init__(self):  
		self._fingers = [Finger() for _ in range(5)]  

def thumbs_up(self):  
	self._fingers[0].flick()
```
- finger objects are created by the hand object
- the hand can keep track and interact with the finger object through the private variable `self._fingers`
- A variable prefixed with a single underscore (e.g., `_variable`) is **conventionally** treated as "protected" or "internal" to a class or module
  
- e.g. aggregation
```python
class Driver:  
	def __init__(self, name):  
		self.name = name  

class Car:  
	def __init__(self, driver):  
		self._driver = driver
```

```python
tim = Driver("Tim")  
my_car = Car(tim)  
my_car = Car(driver=tim) # Also works
```
- in composition, composite objects are usually created when main object is created (in the`__init__` method)
- in aggregation relationships, there are usually methods in the public interface that allow to associate/disassociate objects 
```python
class EvoCar:  
	def __init__(self):  
		self._driver = None  

def start_rental(self, new_driver):  
	self._driver = new_driver  

def end_rental(self):  
	self._driver.charge_rental()  
	self._driver = None  
	
tim = Driver("Tim")  
prius = EvoCar()  
prius.start_rental(tim)  
prius.end_rental()
```

### Inheritance
- inheritance is used when several objects are part of the same family
	- they share similar attributes
	- they share similar behaviours
- arrows point to the "parent" class
![[Pasted image 20241019201239.png]]

```python
class Vehicle:  
	def drive(self):  
		pass  

def stop(self, distance):  
	self._mileage += distance  

class Car(Vehicle):  
""" Car will automatically INHERIT from all attributes and methods of Vehicle """  
	pass  

class SUV(Vehicle):  
""" SUV will automatically INHERIT from all attributes and methods of Vehicle  
But you can add attributes / methods too """  
	is_awd = True
```
- all the attributes and methods of the parent class are inherited by the child class
- if an attribute is defined in the parent class and also in the child class, the child class has precedence
- you can travel up the inheritance tree by using `super()`
- `super()` will give access to the parent class (and its methods/attributes)

#### inheritance limitations
- should be used sparingly
- lots of occurrences where inheritance can be replace with composition
- can be useful when used to create "better versions" of standard python types

#### using inheritance to create abstract classes
- you can use inheritance to define "generic classes" that only define a specific public interface, without implementing it
- the child classes will inherit all attributes from the parent class, and still override the public interface methods

```python
class Animal:  
	def __init__(self, name):  
		self._name = name  

	def sound(self):  
		raise NotImplementedError("Abstract method for animal sound must be implemented by the child class")  

	def show_name(self):  
		print(self._name)  

class Cow(Animal):  
	def sound(self):  
		return "MOO"  

class Dog(Animal):  
	def sound(self):  
		return "WOOF"
```

```python
from abc import abstractmethod, ABC 

class Animal(ABC):  
	def __init__(self, name):  
		self._name = name  

	@abstractmethod  
	def sound(self):  
		pass  

	def show_name(self):  
		print(self._name)
```

### Class Diagrams
- classes and objects have attributes and behaviours
- UML is a common language 
	- UML (Unified Modeling Language) is a standardized visual modeling language used to specify, visualize, construct, and document the structure and behavior of systems, particularly software systems
- encapsulation and abstraction

#### required items in a class diagram
- class diagram shows
	- name of the class
	- attributes and their types
	- the methods, with:
		- parameters and their types
		- the return value and its type

![[Pasted image 20241023155718.png]]

- The `@property` decorator can be confusing: it "looks like" an attribute, but works through methods  
- It is generally located in the methods section of the class diagram (see above)  
- The convention in Python is to use `_name` for a private variable called name .  
- In UML, you use a `-` for private attributes, and `+` for public attributes

### Objects and relationships
- objects do not exist by themselves
	- like their real life representations, they exist in a context
- objects interact with each other
- objects are linked by relationships
- e.g.
	- between two persons A and B
		- A is married to B
		- A is the father of B
		- A is the boss of B
	- between hand and a finger
		- a hand has five fingers
		- a finger belongs to a hand

### Relations in UML diagrams
- represented by a line connecting the two classes
- different types of relationships are
	- **composition**
		- when one object cannot exist without the other
	- **aggregation**
		- when two objects are related but can "exist" independently of each other
	- **extension (or inheritance)**

![[Pasted image 20241023161326.png]]

### Dunder (magic) methods
- everything is an object
- base type (class) in python is an `objecct`
- python defines several built in methods (enclosed by `__` the "dunder" - double underscore)
- `__init__` is the constructor (initializes values)
- dunder methods are "magic" because they are run without being called explicitly 

e.g. 
```python
class Person:  
	def __init__(self, name):  
		self._name = name  

def __str__(self):  
	return f"Person: {self._name}"
```
- `__str__` is called when casting the object into a string

![[Pasted image 20241023162056.png]]
![[Pasted image 20241023162110.png]]

#### Implementing `__lt__` methods for Score class
![[Pasted image 20241023162521.png]]

#### Design pattern: using collections
![[Pasted image 20241023162701.png]]

### using `operator` for sorting (and other things)
- the standard library comes with `operator` module
- provides premade functions to access items/elements of an object/ collection
- `itemgetter`: to get items from list/dictionaries by key (index for a list or key for a dictionary)
- `attrgetter`: to get attributes from objects (by object name)
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

![[Pasted image 20241023163114.png]]

![[Pasted image 20241023163309.png]]

### Advanced testing: fixtures and mocks

#### Fixtures
- for tests, common to create an object and run all the different methods to check their behaviours
- fixtures allow you to define objects that can be reused in different test functions
- It provides setup and teardown functionality, often creating or preparing objects or resources that are needed by tests (e.g., databases, files, or mock data).
- defining a fixture, you make it easy to reuse the same setup code across different tests.
```python
import pytest  

@pytest.fixture  
	def bank():  
		bank = Bank("BCIT Bank")  
		return bank  

def test_bank(bank):  
	assert bank.name == "BCIT Bank"
```
- defined using decorator `@pytest.fixture`
- they are regular functions
- the value returned is the "fixture" you can use in your test function
- to make a test function use a fixture, use its name as an argument in a test function
- Fixtures can be used to create mock objects (e.g., mock API responses) so that tests run in isolation without relying on external services.
- used to set up a consistent and controlled environment for tests

![[Pasted image 20241023164111.png]]

### Mocks and Patching

#### unit tests
- unit tests should only test a specific class or function, and not the dependencies of that code
- we generally use mocks or patch our functions/methods/classes

#### Monkeypatching
- the `monkeypatch` fixture is available in pytest by default 
- use it in your test functions to change the behaviour of certain objects or classes
	- similar to `setattr`
- It's mainly used to mock dependencies, control behavior, and isolate functionality without changing the original code.
- **Temporarily modifies** objects, functions, or modules in the code
- Typically used to **replace existing behavior** in a function or class with a custom implementation for the test’s duration.
- in the example, we want to monkypatch `balance` attribute for the BankAccount class
![[Pasted image 20241023164748.png]]
![[Pasted image 20241023165229.png]]

#### when to use
- **Fixture**: Use when you need to **prepare a test environment**, such as setting up a database, creating sample data, or configuring some other external resource that the test relies on.
- **Monkeypatching**: Use when you need to **mock or override a specific part** of the code (like a function or environment variable) during the test.

#### Mocks
- mocks are pieces of code that replace the behaviour of another piece of code
- the process of "injecting" mocks into code is called **patching**
- used to simulate the behavior of real objects or functions in a controlled way
- **Mocks** are objects or functions that imitate the behavior of real components during testing
- can be programmed to return specific values or raise exceptions when called, and they allow you to test how your code interacts with these external dependencies
- - **Mocks** focus on _mocking specific methods or interactions_ to control test behavior and verify function calls.
- **Fixtures** set up a _test environment_ (like a database connection) without necessarily replacing components or functions in your code.

#### patching with `patch`
![[Pasted image 20241023171103.png]]
- the code patches the builtin `input` method
- it replaces it with a mock
- the mock has a side effect
	- it returns "abc" the first time it is called
	- and returns "def" for the second time it is called
- the `mock_input` replaces the `input` method, makes it non-interactive and predictable

#### patching the `open` function
- common to patch the `open` method in python
- the tests should not depend on local files whose content you don't control
- unit tests will patch `open` to control the "content" on which the program operates 
- It can be complicated to create a mock object every time - especially because open is a standalone function, but can also be used as a context manager ( with open [...] ).  
- Python comes with a preexisting mock called mock_open .  
	- Use it in combination with patch

![[Pasted image 20241023171653.png]]

### Webscraping 
- httpx (to make HTTP requests)
- beautifulsoup4 (Beautiful Soup, to parse HTML pages)
- `pip install httpx beautifulsoup4`

![[Pasted image 20241023171836.png]]
- HTTPX is a module used to make HTTP requests

![[Pasted image 20241023171908.png]]
- `BeautifulSoup` is a module that allows you to parse and process HTML documents
- `soup` becomes a complex tree of objects, representing HTML elements in the document (DOM tree)

#### Navigating and searching the tree 
- To find the first element of a given tag, you can use `.find`
	-  For example, the first H1 tag can be found with: `element = soup.find("h1")`
	- You can then extract the text for the HTML tag by looking at its text attribute: `print(element.text)`
	- If the element has children, you will find them in `.children`
- you can also use CSS selectors to get finer and better control over the tree
	- e.g. the price of a book can be found with: `price = soup.find(".product_main p.price_color")`
- selectors return a list of elements
	- you can use `[0]` to get the first one

![[Pasted image 20241023191307.png]]

```python
class Book: 
	def __init__(self, url): 
		response = httpx.get(url) 
		soup = BeautifulSoup(response.text, "html.parser")
		self.title = soup.find("h1").text # etc.
```
- Find the text of the sibling to element : `element.next_sibling.text`
- Get all the classes of an HTML element: `element.attrs["class"]`
#### Regular Expressions
- you can use regular expressions to do pattern matching
- `\d` : matches a digit 
- `\s` : matches a whitespace (tabs, spaces, etc) 
- `\S` : anything that is not a whitespace (see above) 
- Patterns also contain 'quantifiers'. The only quantifiers you need to know at this stage are: 
- `?`: present 0 or 1 times. For example .jpe?g matches .jpg and .jpeg , but not .jpeeeg .
- `*`: present 0 or more times. .`jpe*g` matches all the above examples. 
- `+` : present 1 or more times. .jpe+g matches .jpeg , .jpeeeg but not .jpg .