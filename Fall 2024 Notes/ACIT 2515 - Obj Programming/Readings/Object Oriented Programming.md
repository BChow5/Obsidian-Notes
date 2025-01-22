- <mark style="background: #ABF7F7A6;">Class</mark>
	- defines a general category (i.e. book, bank account)
	- blueprint (or template) for creating an object
	- a custom data type 
- <mark style="background: #ABF7F7A6;">object or instance</mark>
	- a "thing" created out of a class
- <mark style="background: #ABF7F7A6;">attributes</mark>
	- values for a specific object
- <mark style="background: #ABF7F7A6;">methods</mark>
	- behaviours (or capabilities) of a class
- <mark style="background: #ABF7F7A6;">state</mark>
	- the current values of the attributes in an object 

```ad-example
title: Understand the diff between class and instance
- a book is a class = abstract concept of something you can read
- "Harry Potter" that you bought is an instance of a book
	- it is one specific, concrere book object that follows all the "laws of books"

```

### Object Oriented Programming

#### Objectives
- modular design and reusable code
- data protection
- driven by objects and behaviours rather than programming logic

#### Important Concepts
- modelization
- encapsulation
- abstraction
- inheritance
- polymorphism 

#### Modelization
- the objective of a model is to "describe" an object
- how can we describe a person?
- we represent the objects by creating a <mark style="background: #ABF7F7A6;">model</mark>, and implement it using a class 

#### Encapsulation (and visibility)
- the state of an object should only be changed by the object itself
- use behaviours (methods) to alter the state, instead of changing the attributes
- some object oriented programming languages have a visibility features where you can make attributes private
	- only the object itself can change them
- does not exist in Python but there is a convention where you use `_` in the beginning of the attribute to mean private
- using private attributes and using methods to change the state is called <mark style="background: #ABF7F7A6;">encapsulation </mark>

#### Abstraction
- the implementation of the behaviours of the object is the responsibility of the class
- other objects should not depend upon private attributes or methods
- they should only use the public interface of the class: the public methods and attributes 

#### Class Syntax in Python
- `class MyClassName:` to define a new class "block"
- indent the code below
- except for class variables, a class definition only has methods
- methods are defined inside the class block, and receive `self` as first parameter
- a special method is `__init__` always called upon initialization of the instance 

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

####  The self parameter
- all functions defined within a class are called "<mark style="background: #ABF7F7A6;">methods</mark>"
- they receive an implicit parameter `self` which represents the current instance
- you NEVER use `self` outside of the class block
- `john.display()` calls the method display on the object john .  
	- In the display method, `self` is equal to `john` !  
-  `bob.display()` calls the method display on the object bob .  
	- In the display method, self is now equal to bob

#### Parameter Validation
- imagine the `student` class should not allow students with "invalid names"
- we will make it raise an exception if that is not the case

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

#### Using Python `Property`
- Python has a decorate which makes encapsulation and abstraction easier 
- you can use the `@property` decorator on a method, and then it will behave as an attribute 
- this allows you to implement getters very easily 

```python
class Student:  
	[...]  
	@property  
	def info(self):  
		return f"{self.name} ({self.student_number})"  
john = Student("John Doe", "A01234567")  
# Note how we use info as if it were a variable!  
# We don't use () even though info is a method.  
print(john.info)
```

#### Python Properties: the setter
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
- you can only create a setter when you have a getter defined (property)
- the getter and setter methods are called the same name. you also need to use this name in the `@[name].setter` decorator 

#### Static methods and class methods
- you can define methods that are common to all objects of the same class
- static methods do not get any reference to the class or object
- class methods receive a reference to the class when called directly from the class name 
- this reference is typicalled called `cls`
- use the `@staticmethod` decorator for the statc methods (no reference)
- use the `@classmethod` decorator for class methods (reference to the class)
  
#### Static method example
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

#### Class method example
```python
class Student:  
	PROGRAMS = ("CIT", "CST", "BTech")  
	[...]  
@staticmethod  
	def check_program(cls, value):  
		# This is a static method. It does not receive an implicit argument.  
		# It does not have access or know about the class / instance  
		return value in cls.PROGRAMS  

Student.check_program("CIT") # will return True  

john = Student("John Doe", "A01234567")  
john.check_program("CIT") # will also return True

```

#### Class variables
- class variables are attributes that are common to all objects of the same class
- they are defined in class block, but outside any methods
- the previous example uses a class variable called programs
- class methods have ass to class variables but not instance variables
- static methods do not have access  to class variables, nor instance variables 

#### What you really need to know 
- How to define a class: class MyClass:  
- Understand what the constructor does ( `__init__` )  
- Understand what self is  
- Be able to write any instance methods  
- Because you define an instance variable self.something in `__init__` ...  
- DOES NOT MEAN that `__init__` will take it as a parameter!  
- Understand what inheritance, but also understand when NOT to use it (i.e. most of the time by yourself, unless   you are using a library / package)  
- In the context of this course, using class variables and class / static methods and variables is a bad idea and almost never happens, except when working with SQLAlchemy. If you are using them, make sure you know what you are doing.  
- There is very little to learn or know about Object Oriented Programming. It is mostly about how you "view the world" in your program.