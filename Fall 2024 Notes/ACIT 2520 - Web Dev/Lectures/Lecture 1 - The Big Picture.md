* CPU is only capable of understanding binary
* CPU is brain of computer
* Code reaches the CPU through series of translations 

### Assembly
* Solution to binary being hard to read
	* assembly was created
* we feed our assembly code into an assembler
	* the assembler is just another program on the computer
	* translates our assembly code into binary for computers 

### C Language
* moving up another layer of abstraction
	* us finding easier ways to rationalize the code we're looking at
* C language requires a compiler
* Compiler is a program that converts our source code to Assembly
	* goes through all of our code and then produces a version of assembly
* pros:
	* our program doesn't get executed by CPU until the compiler finishes going through all the code top to bottom
		* compiler can find errors and warn us ahead of time
* cons:
	* need to wait every time for compiler to check

### Interpreters
* alternative to compiler
* performs real time translations of code
* line by line read each line of code 
* don't need to wait for the whole program to be read
	* however, if any error or bugs, it will keep sending it and it will cause a crash
* JavaScript uses an interpreter 

### Browsers use "languages"
- HTML
	- content
- CSS
	- appearance
- JavaScript
	- controls behaviour

### What does it take to create a "program" running?\
* 2 things:
	* program language
		* could be like Python, JavaScript
	* run time environment 
		* compiler or interpreter
		* provide extra facilities to help your programming language interact with its environment
```ad-example
title: Java Runtime Example
e.g. we are a developer of Zoom

![[Pasted image 20240905192515.png]]

```

### JavaScript
* Since JavaScript is designed for the browser, it uses the browser itself as the runtime environment 
```ad-example
title: JavaScript Runtime Example
![[Pasted image 20240905193058.png]]

```
* browsers use web APIs to add more functionalities to JavaScript
	* they aren't actually part of JavaScript language 

#### Problem with JavaScript
* problem is that runtime environment is only in the browser
* if we don't have browser or access to one, we can't run JavaScript
* when executed in browser, it is sandboxed into the browser
* why is this a problem?
	* no ability to communicate with users hardware
	* cannot store large amounts of information or save information into a database
	* cannot store sensitive information
* to resolve this problem, NodeJS was made
	* allows JavaScript to run outside of the browser

#### NodeJS
* NodeJS won't have access to web APIs
* for JS, we need to explicitly say when we want to use a module
	* module = code written by someone else
* e.g. 
	* var os = require("os");
* the Node REPL is when we've just gone into the Node interpreter and can explore what Node is capable of doing by getting immediate feedback for the Javascript we type 
	* we can enter the node REPL by typing Node into the console 

##### Process Module
* process.argv lets us send information into our program 
	* not actually a function
	* it is a property
* gives the location of where node is on out computer and location where the current script is running from 