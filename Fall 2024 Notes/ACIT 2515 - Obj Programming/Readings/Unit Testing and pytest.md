### Why Test?
- unit tests are automated tests
- unit tests operate at the lowest level possible
	- only validate the basic elements of a program
- unit tests do not validate how objects interact with each other (this is integration testing)

### Unit Tests
- fast to run (usually few seconds)
- east to write (when written at the same time as the code)
- can be automated and scheduled 
- unit  vtests can also help developers
	- readinf the tests tells a lot about the public interface and its model
- <mark style="background: #ABF7F7A6;">Test Driven Development (TDD)</mark> - method where you write tests before/at the same time as writing the actual program 

### Testing in Python
- the testing framework provided by Python is called **unittest**
- it is derived from **jUnit** and has a java-oriented syntax
- good to know about unnittest but **pytest** is more convenient to use

##### Install pytest
- pytest is a python package
- install it in your virtual environment **pip install pytest**

#### Run tests with pytest
- at the root of your project, run pytest
- pytest will scan and discover all your test in the test files
- test files are any files that match the patterns:
	- **test_* .py**
	- * _ test.py
	- these files can be subdirectories/folders
- you can also just run it with the command pytest
- tests are functions/methods whose names start with test
- tests can be
	- regular functions outside of classes
	- methods inside classes whose names start with test
- usually tests located in the test folder

```ad-example
![[Pasted image 20240906190542.png]]

```

#### How to write a test
![[Pasted image 20240906190614.png]]

#### How to test for exceptions?
- It is sometimes expected that your code raises exceptions
- ![[Pasted image 20240906190702.png]]

##### The test
![[Pasted image 20240906190729.png]]

#### Importing test to the test file
![[Pasted image 20240909093312.png]]
- if we want to test multiple functions, you will list all of the functions in the import
- or if we want to just do *import monday*
	- you will need to do *monday.add* whenever you call the function

#### Code coverage and unit testing 
- the goal of unit testing is to make sure every aspect of the code is tested and working
- we can measure "how much code" is covered by unit tests - the <mark style="background: #ABF7F7A6;">Code Coverage</mark>
- it is typical for teams to aim for at least 80% of code coverage
- reference - [5 questions every unit test must answer ](https://medium.com/javascript-scene/what-every-unit-test-needs-f6cd34d9836d)

#### Code coverage in Python
- we can use the **coverage** library (an external dependency) together with pytest
- you can install it in a virtual environment with **pip install pytest-cov**
- you can run your tests and add coverage with the **--cov** option
- the **--cov** option can be:
	- a python module: **pytest --cov-add**
	- a python package (use . for the current folder): **pytest --cov-.** 
- **coverage** will report about all the python files used in your program

#### Coverage reports
- generate a nice looking HTML report: **pytest --cov=. --cov-report=html**
- This will create / update contents in the htmlcov folder. You can open the index.html in your favorite browser