```ad-warning
title: Running Your Code
- You should run your code with "python ..." in the terminal
- Do not use the **Play** button in your editor unless you know what you're doing

```

### Variables
* declare, assign, set a variable (save a value) 
	* *my_variable = "something"*
* variables defined inside a function are ownly known inside that function
* global variables are bad
	* use functions, arguments, and return values instead 
* Python variables should follow the snakecase convention ( my_great_variable , not myGreatVariable )
* Constants should be uppercase: 
	* *NUMBER_OF_ATTEMPTS = 2*  
* All variables have a type: type(my_variable)

### Numeric Types
* round_number = 20  
* float_number = 42.4  
* string_number = "100"  
* int(float_number) # 42  
* int(string_number) # 100 (not '100' !)  
* float(round_number) # 20.0
* Types: int , float , complex  
* Can do arithmetics: a + b , a - b , a * b , a / b  
* Integer division and modulo: 14 // 3 == 4 , 14 % 3 == 2 (14 = 4 * 3 + 2)  
* Complex math are done with import math  
	* math.sqrt , math.pow , math.log

### Booleans: Bool
* use == to check if two values are equal
* != to check if they are different
* = assigns a value to a variable
* boolean variables can only be True or False
	* you can combine with *and, or,* and *not*
```ad-caution
Remember that not (A and B) is not A or not B , and is not the same as not A and not B

```
* *none* is also a built in value

### Conditions: if
```ad-example
if condition:  
	(runs if condition is True)  
elif another:  
	(runs if condition is False and another is True)  
else:  
	(runs if condition is False and another is False)  
	
There is also a short version (one-liner) for if statements.  

* can_drink = True if age > 19 else False

```

### Collections
* Contain multiple elements 
* Elements can be different types
* list, tuple, set, dict

#### Properties of standard collections

| Collection | Ordered (sortable) | Comments                     |
| ---------- | ------------------ | ---------------------------- |
| **List**   | yes                | mutable                      |
| **tuple**  | yes                | not mutable                  |
| **set**    | no                 | mutable. elements are unique |
| **dict**   | no                 | key value pairs              |

#### Lists and tuples
- a list is a collection of ordered elements (= sequence)
- Type is list or tuple
- Sequences in Python are ordered (you can sort them)
- Lists can be mutated
	- you can change the values of the elements
- tuples can NOT be mutated
	- you cannot change their values once defined
- sequence[index] accesses the element at index in sequence .  
- append vs extend , insert , copy , pop , reverse
- [Common sequence operations](https://docs.python.org/3/library/stdtypes.html#common-sequence-operations)
- [Operation on Mutable Sequences](https://docs.python.org/3/library/stdtypes.html#mutable-sequence-types)
-  ![[Pasted image 20240904183605.png]]

#### Sets
![[Pasted image 20240904183656.png]]

#### Iterate on collections
* use *for variable in collection*
	* ![[Pasted image 20240904183751.png]]
* do not iterate with indexes
```ad-warning
title: Avoid the following
![[Pasted image 20240904183834.png]]

```

#### Useful functions/properties
![[Pasted image 20240904183916.png]]

#### Dictionaries
* Type is Dict
* very useful data type
* maps keys to value
* dictionaries are not ordered
* you can NOT sort a python dictionary
* *dictionary[my_key]* access the element key *my_key* in *dictionary*
* has methods:
	* keys()
	* values()
	* items()
	* update
* [mapping types](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
* ![[Pasted image 20240904184302.png]]

#### iterate on dictionaries
![[Pasted image 20240904185043.png]]

### Strings (text)
* Strings are like a list of characters
* in Python, strings are immutable (you cannot change a sting. only make a copy with changes)
* Type str 
	* defined with single, double, or triple quotes
* useful things for strings:
	* split, join, upper, lower, isnumeric
* use f-string
	* my_string = f"Hello {name}, nice to meet you!"
	* "abc de f".split -> ["abc", "de", "f"]
	* " ".join(["abc", "de", "f"]) -> "abc de f"
* [common string operations ](https://docs.python.org/3/library/string.html)
* string methods
	* ![[Pasted image 20240904185419.png]]

#### Iterate on strings like lists
![[Pasted image 20240904185457.png]]

#### Convert strings to numbers and vice-versa 
![[Pasted image 20240904185543.png]]

### Functions
* functions have parameters and receive arguements
* they return a value (of any type)
```ad-example
![[Pasted image 20240904185635.png]]
* The function register_student takes four parameters (or arguments):
* student_id , and name must be provided in this order. They are positional arguments.
* international and scholarship are keyword arguments. They do not have to be provided, because they have a default value
* keyword arguements can be provided in any order by using their names


```

#### Return values
* all functions return something using the return statement
* if the return statement is omitted, the function will return none
* a function returns ONE value but the value can be a collection (list, set, tuple, dictionary, etc.)

### Exceptions
* errors in the code raise exceptions
* you can also manually and voluntarily raise an exception
* you can cause exceptions using *try ... except*
* ![[Pasted image 20240904190125.png]]
* avoid using "empty" try/except statements (without specifying the exception, as they make it hard to debug and are bad practice)

### Files and paths
![[Pasted image 20240904190224.png]]
* always use *with* or remember to close the file
* 99% of the time, you should use **relative path**
	* Absolute paths always point to the same location regardless of the current directory, whereas relative paths may point to different locations depending on the current directory.
* open("C:\\Users\\tguicherd\\Documents\\ACIT2515\\lab.txt") is most likely wrong (absolute path).  
* open("/home/users/tguicherd/lab.txt") is most likely wrong (absolute path).  
* open("../data/file.txt") is suspicious. Are you sure you want to go up the directory tree?  
* File extensions do not matter: whatever.docx saved from Word is actually a ZIP file.  
* File formats are a convenience. A file is a file.  
* It is common to distinguish text files (contains data that can be printed / read on the screen) and binary  Files (contains data that are not characters).  
	* Still, there are just files.

### Python modules and import
![[Pasted image 20240904191033.png]]

