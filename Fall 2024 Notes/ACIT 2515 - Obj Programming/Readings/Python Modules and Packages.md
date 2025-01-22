### Built in packages
* we already have used packages in Python
* e.g.
	* import random
	* import math
	* import string
	* from collections import Counter

### Important concept
* Python code is written in files
* Each file is a python module
* you can import other files, and/or import only specific symbols of a module 
* let's start with a very simple module: my_print.py
```ad-example
title: my_print.py Example
![[Pasted image 20240904200750.png]]

```

### Import a module from another module
* We can import this file (module) using *import*
* let's write another module that uses my_print
```ad-example
title: my_print Example
![[Pasted image 20240904200908.png]]

```
### Import only specific variables/functions 

![[Pasted image 20240904200936.png]]

### Important
* the entire module *my_print* is imported 
* you can access functions or variables using the "." notation 
* everything in Python is an object
* you can access "properties" of the module like you access "behaviours" for data types (with the .)
* when the module is imported, the code it contains is EXECUTED 
* make sure you use *if _ _name_ _ == "_ _main"* to prevent side effects

## Python Packages
* packages are a collection of modules
* several files in a folder
* a Python package is a folder with a _ _ init _ _ .py file in it
	* packages can work without an explicit _ _ init _ _ .py file but you should always have one
	* explicit is better than implicit 

### About Packages
* Python packages can contain modules, and other packages
![[Pasted image 20240904203205.png]]

### Importing modules and packages
* You can import the whole package like you did with modules: import logic
* and then, use . to access submodules/packages
![[Pasted image 20240904203339.png]]

#### Importing sub packages
![[Pasted image 20240904203420.png]]

#### Importing and renaming
* can be convenient for long/complicated names
![[Pasted image 20240904203446.png]]

### Absolute and relative imports 
* by default, Python will look for modules and packages:
	* in the current folder
	* in the folder of your virtual environment/python installation
* it is sometimes desireable to tell Python to look for modules and packages in paths relative to the current modules path
* in that casse, you must import the module/package by adding a . at the beginning
	* relative imports can be tricky. use them only if you know what you are doing or in __ init __ .py files

### Using __ init __ .py to allow easier access to subpackages 
* when Python encounters an import statement and the symbol imported is a package, the __ init __ .py file will automatically be fun
* the file can import functions/variables from submodules and packages to allow for easier imports
* ![[Pasted image 20240904205013.png]]
* ![[Pasted image 20240904205059.png]]

### Packages and paths
* When running a Python program, the working directory is the directory where you ran the Python command
* this can have an effect when testing/developing your programs
* ![[Pasted image 20240904205151.png]]

#### Using the CLI and sys.argv
![[Pasted image 20240909111356.png]]
- there is always 1 element in *sys.argv* by default
	- the default element is the name of the time 
	- so when indexing, you need to start at 1 for the first inputted element 
- ![[Pasted image 20240909111748.png]]