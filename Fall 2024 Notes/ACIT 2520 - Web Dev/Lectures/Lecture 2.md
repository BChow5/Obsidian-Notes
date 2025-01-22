- Web API's only works in JavaScript on the web
- NodeJS API - only work in NodeJS
	- e.g. os.cpus()
- avoid global variables as much as possible 
	- use local variables instead 
- do NOT use var to make variables in JS
	- using var will make them a global variable 
- use *Const* or *let* to get your variables local 
- always use const to declare your variables

#### Mutation
```ad-example
title: Mutation Example
![[Pasted image 20240909141814.png]]

```
- this is an example of a mutation
- you can mutate a list like this
- but you cannot reassign it to a new list in memory 

#### Objects
-  for JS, we use the term object instead of dictionary 

#### Functions
- Functions can be called directly or indirectly
- ![[Pasted image 20240909143019.png]]

##### Higher Order Functions and Callback Functions
- ![[Pasted image 20240909144611.png]]
	- addTwo in this example is a special function
		- it is a function that takes at least 1 other function as a parameter
		- <mark style="background: #ABF7F7A6;">Higher Order Function</mark>
	- so we are actually calling the add function in here
	- the generic name for the addReference in this example is <mark style="background: #ABF7F7A6;">Call Back Function</mark>

```ad-example
![[Pasted image 20240909145418.png]]
- the callback function won't actually be called callback but it is the generic name
```
- Callback functions should only be called by Higher Order Functions
	- we want to prevent our Callback function from being called by anyone other than the Higher Order Function

###### How do we prevent anyone else from using the Callback function?
![[Pasted image 20240909152538.png]]
- ![[Pasted image 20240909152858.png]]
	- best practice to use the arrows to declare the function when we're doing Callback functions 

##### Function Expression vs Function Hoisting
- Hoisting would move your functions to the top to run them
	- this was considered bad because it was moving your code
- Function Expression 
	- prevents hoisting 
	- const sub = function (n1,n2)
- Arrow Function Expression
	- *add = (num1, num2) => {return num1 + num2};*

#### Loops

##### For of Loops
- use the *For of* loop most of the time
- ![[Pasted image 20240909153852.png]]

##### forEach Loop
- forEach is a Higher Order Function and it uses a callback function
- the callback function receives a value 
- ![[Pasted image 20240909154122.png]]
	- this will print out all the colors in the list 
- ![[Pasted image 20240909154200.png]]
	- we can add in i to get the position of the colors too 
```ad-example
title: Example of Callback and Higher Order Function
![[Pasted image 20240909154313.png]]

```
![[Pasted image 20240909160102.png]]

#### Reading Files
![[Pasted image 20240909161620.png]]
-  the *.toString()* indicates that the information should be human readable 

![[Pasted image 20240909162535.png]]
- compared to the earlier example, we change from *readFileSync* to *readFile* and now it needs to take in a callback function
	- when reading the files is complete, it will then run the callback function
- <mark style="background: #ABF7F7A6;"> asynchronous programming </mark>
	- gives the benefits of multithreading
	- the benefit we can get from having callback functions in our coding 
	- important for building fast website 
- nodeJS will know that it may take a while to read the files, so it will have it run in the background and continue to run the rest of the program