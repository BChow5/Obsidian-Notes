### Fixtures
- in tests, it is common to create an object and run all the different methods to check their behaviours
- each test function will test a specific feature of the class
- you may end up creating the same object over and over in all of your test functions
- <mark style="background: #ABF7F7A6;">fixtures</mark> are meant to solve this problem
	- they allow you to define object that can be reused in different test functions

```python
import pytest  
@pytest.fixture  
def bank():  
	bank = Bank("BCIT Bank")  
	return bank  
	
def test_bank(bank):  
	assert bank.name == "BCIT Bank" 
```

#### Pytest Fixtures
- define using the decorator `@pytest.fixture`
- they are regular functions
- the value returned is the "fixture" that you can use in your test functions
- to make a test function use a fixture, use its name as an agreement in a test function 
```python
import pytest  
@pytest.fixture  
def tim():  
	return Customer("Tim", "A0001")  
@pytest.fixture  
def checking(tim):  
	return CheckingAccount(tim)
```
- you can also "nest" fixtures

### Mocks and Patching

##### The problem we're trying to solve

```python
class BankAccount:  
	[...]  
	@property  
	def balance(self):  
		""" Return the balance of the account """  
	[...]  

class BankCustomer:  
	def __init__(self):  
		self._accounts = list()  
	def add_account(self): [...]  
	def total_balance(self):  
		return sum([account.balance for account in self._accounts])
```
- How to test BankCustomer without testing BankAccount at the same  time?
#### Unit Tests
- unit tests should only test a specific class or function, and not the **dependences** of that code
- there are some elements that you do not need to isolate
	- you don't have to test if `super()` works, or if a `list` is really a list, for instance
- there are different ways to make it happen. in Python, we generally use **mocks** or **patch** our functions/methods/classes

#### Example: broken properly in `BankAccount`

```python
def test_bank_customer_total_balance():  
	tim = BankCustomer("Tim")  

	account = BankAccount("Test", 1000)  
	tim.add_account(account)  

	assert tim.total_balance == 1000
```
- let's imagine there is a bug in the `balance` property of the BankAccount class
	- this test will fail, because of the big in another class
- we need to remove the dependency between BankCustomer and BankAccount

#### MonkeyPatching
- the `monkeypatch` fixture is available by default in Pytest
- you can use it in your test functions to change the behaviour of certain objects or classes 
- it is used very similar to `setattr` in Python
- in our case, we want to mokneypatch the `balance` the attribute for the BankAccount class
- we are going to monkeypatch it with a standard integer 
```python
def test_bank_customer_total_balance(monkeypatch):  
	tim = BankCustomer("Tim")  
	account = BankAccount("Test", 1000)  
	tim.add_account(account)  
	
	monkeypatch.setattr(BankAccount, "balance", 1000)  
	assert tim.total_balance == 1000
```
- this allows the test to pass without fixing the code in a different Python module
- note that the `monkeypatch` is a fixture received as argument to the test function 
- you can use <mark style="background: #ABF7F7A6;">monkeypatching</mark> to replace that function with a mock that returns predefined data for testing purposes.

#### Mocks
- monkey patching is very useful and efficient in simple cases
- it allows you to change attributes on the fly to make your test pass
- sometimes it is not possible to monkey patch specific attributes, and you need to replace your entire class
- sometimes you may want to change the behaviour of "builtin" functions such as `input` or `open`
- mocks are used in this case. they are pieces of code that replace the behaviour of another piece of code
- the process of "injecting" mocks into the code is called <mark style="background: #ABF7F7A6;">patching </mark>

#### Patching the input method
```python
@patch("builtins.input", side_effect=["abc", "def"])  
	def test_example(mock_input):  
	value1 = input()  
	value2 = input()  
	assert value1 == "abc"  
	assert value2 == "def"
```

#### patching with patch
- the code patches the builtin `input` method
- it replaces it with a mock (available in the test function as `mock_input`, see arguments)
- the mock has a side effect
	- it returns "abc" the first time it is called
	- and returns "def" the second time it is called
- the `mock_input` replaces the `input`method. it makes it non-interactive and predictable 

#### patching the `open` function
- very common to patch the `open` method in Python
- the tests should not depend on local files whose content you don't control
- unit tests will patch `open` to control the "content" on which the program operates
- it can be complicated to create a mock object every time - especially because `open` is a standalone function, but can also be used as a context manager (`with open [...])
- python comes with a preexisting mock called `mock_open`
- use it in combination with `patch`

#### Python with `mock_open`
```python
from unittest.mock import mock_open, patch  
FILE_CONTENTS="line1\nline2\nline3\n"  
@patch("builtins.open", new_callable=mock_open, read_data=FILE_CONTENTS)  
def test_open(mock_file):  
	with open("my_file.txt", "r") as fp:  
		data = [line.strip() for line in fp.readlines()]
	  
	assert data[0] == "line1"  
	assert data[1] == "line2"
```