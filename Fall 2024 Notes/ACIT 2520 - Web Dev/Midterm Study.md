## Fundamental JS Knowledge

### Variables
- define a variable: `const var1 = value`
	- best to use const rather than let
- **DO NOT** use `var` to declare variables
	- var will make variables global 
- Variables declared with **`let`** and **`const`** are block-scoped, meaning they are confined to the nearest enclosing block (e.g., function, loop, or condition block).
- use `===` instead of double equal for comparing
	- == does an implicit type conversion 

### Functions
- function declaration
	- A function declaration defines a named function and can be hoisted, meaning it can be called before it is defined.
```javascript
function functionName(parameter1, parameter2) {
  // function body
  return parameter1 + parameter2; // Example
}
```

- function expression
	- A function expression defines a function inside an expression and assigns it to a variable. It can be anonymous or named.
```javascript
const add = function(parameter1, parameter2) {
  return parameter1 + parameter2;
};

console.log(add(5, 10)); // 15
```

- arrow functions 
	- shorter syntax and are often used for inline functions
```javascript
const add = (parameter1, parameter2) => {
  return parameter1 + parameter2;
};

console.log(add(5, 10)); // 15
```

### Loops
- `for...of` loop
```javascript
let fruits = ['apple', 'banana', 'cherry'];
for (let fruit of fruits) {
    console.log(fruit);  // prints each fruit name
}
```

- `for...in` loop
```javascript
let person = { name: 'Alice', age: 25, city: 'Vancouver' };
for (let key in person) {
    console.log(key, person[key]);  // prints the key and its value
}
```
- for loops: `for (const element of object) {}`
	- useful when you just need the **values** from the iterable (rather than the keys or indices).
	- most commonly used loop for us

#### .forEach()
- `.forEach()` iterates over an array of elements and executes a provided function once for each array element
```javascript
array.forEach(function(currentValue, index, array) {
  // code to execute on each array element
});
```
- does not return a new array 

### Arrays and Objects

#### Arrays
- creating arrays:
```javascript
let numbers = [1, 2, 3, 4];
```
- Array methods: `.push()`, `.pop()`, `.shift()`, `.unshift()`, `.map()`, `.filter()`, `.forEach()`.
- `colors.push("yellow")`
	- adds to an array
- accessing elements:
```javascript
console.log(numbers[0]);  // Output: 1
```

- `forEach()`: This array method executes a function once for each array element.
```javascript
let numbers = [1, 2, 3, 4, 5];
numbers.forEach(function(number) {
    console.log(number);  // prints each number
});
```

- The `map()` method creates a new array by applying a function to each element of the original array. It doesn't modify the original array.
```javascript
let numbers = [1, 2, 3];
let doubled = numbers.map(number => number * 2);
console.log(doubled);  // prints [2, 4, 6]
```
#### Objects
- creating objects
```javascript
let person = {
    name: "John",
    age: 30,
    greet: function() {
        return "Hello " + this.name;
    }
};
console.log(person.name);  // Output: John

```
- `person.hairColor = "black"`
	- insert into an object 
- `person["age"] = 25;`
	- inserting key and value into object
-  `Object.keys(person)`
	- gives a list of all the keys in the object
- `Object.values(person)`
	- gives a list of all values in the object

```javascript
for (const key in person) {
	const value = person[key]
	console.log(key, value)
}
```
- use `for - in - ` to look through an object 
- only use the `for - of - ` loop for an array. not an object

## Week 1

### Terms
- **compiler**: 
	- a program that converts our entire source code to assembly
	- it goes through all of our code then produces a version of assembly
- **interpreter** 
	- a program that reads and executes JavaScript code line by line, without requiring the code to be compiled into machine code beforehand
	- alternative to compiler 
- interpreters/compilers are translators
- **run time environment** 
	- compiler or interpreter
	- the environment in which the code runs after it has been compiled or interpreted
	- provide extra facilities to help your programming language interact with its environment
	- An interpreter is part of the **runtime environment** for interpreted languages
	- Even after code is compiled, it still needs a runtime environment to execute, as this environment provides the memory, system resources, and execution context
	- run time environment is how we actually run the code we wrote in our programming language 
	- need the translator still
		- turns our JS code into machine language
	- JavaScript relies on APIs and libraries for additional functions provided in the runtime environment
	- runtime environment will already have the translator inside it
- **ecmascript**
	- specification PDF
	- NOT a programming language
	- tells us what JS as a language is capable of doing 
	- only talks about functions purely in JS, so nothing from NodeJS is shown here
### JavaScript
- JavaScript is our programming language 
- JavaScript is both interpreted and compiled
	- uses the v8 engine for browser
- when executed in browser, it is sandboxed into the browser
- JS original runtime environment is only in the browser
	- we made NodeJS to solve this

### NodeJS
- NodeJS is a runtime environment for JavaScript 
	- allows JS to run outside of the browser
	- built on V8 engine
- NodeJS is technically both a interpreter and compiler 
- includes the V8 engine (which both compiles and interprets JavaScript code) and provides access to system resources (file system, network operations, etc.).
- NodeJS doesn't have access to web APIs
- need to explicitly say when we want to use a module from NodeJS

## Week 2

### Asynchronous Functions 
- synchronous functions 
	- execute sequentially where each operation must complete before the next beings
	- program will wait for a synchronous function to finish executing before moving onto the next code
	- this is bad if we're doing a function that will take a long time to finish, making the whole program wait
- asynchronous functions
	- allows the operations to be executed independently of the main program flow
	- when an asynchronous function is called, the program can continue executing the next lines of code without waiting for the asynchronous operation to complete
	- good because it allows other code to run while waiting for a task to complete
		- good for potentially long things like reading or writing files 
- without sync, we will need to add a callback function
	- so that it doesn't do the next function step until after the first is completed 
- if a function is doing something with the fs module, it can be asynchronous 
- T or F if you have 2 asynch operations, should they always be nested?
	- F - we don't always need to nest them
		- only nest when the second operation depends on the first one 
- **2 fundamental problems** for asynch callbacks'
	- readability issue
	- no centralized error handling 
	- this is why we moved to promises

##### Function Expression vs Function Hoisting
- Hoisting would move your functions to the top to run them
	- this was considered bad because it was moving your code
- Function Expression 
	- prevents hoisting 
	- const sub = function (n1,n2)
- Arrow Function Expression
	- *add = (num1, num2) => {return num1 + num2};*

### Callback functions
- having a callback function does not make it asynchronous 
- a function that is passed as an argument to another function and is executed after the completion of that function
	- we usually used it for error handling in class 
- callback allows you to ensure that a function is not run until another function has finished its task
- callback functions should only be called by higher order functions 
![[Pasted image 20241024171840.png]]
- **Synchronous callbacks**: The callback is executed immediately once the main function completes.
- **Asynchronous callbacks**: The callback is executed later, usually after some asynchronous operation is completed (e.g., reading a file, making a network request).
## Week 3+

### Promises
- promises are objects that represent the eventual completion (or failure) of an asynchronous operation and its resulting value
```javascript
new Promise(() => {})
// it needs a callback function passed into it


new Promise ((resolve, reject) => {
    resolve(); // how to change the promise to fulfilled
});


new Promise ((resolve, reject) => {
    reject(); // how to change the promise to rejected
});
```
 - promises have 3 statuses
	- fulfilled
	- rejected
	- pending
- use `promise.resolve()` if you need to call a function for your next step but that function doesn't return a promise
	- `promise.resolve()` will make an empty promise chain
- use `const fs = require("node:fs/promises");` to make all `fs` functions into promises 
- `.then()`: Used to specify what to do when the promise is fulfilled.
- `.catch()`: Used to specify what to do when the promise is rejected.
- `.finally()`: Used to specify a block of code that will execute regardless of the promise's outcome.

### Writing our own promises
![[Pasted image 20241024174138.png]]

```javascript
const myPromise = new Promise((resolve, reject) => {
    // Asynchronous operation
    const success = true; // Example condition
    if (success) {
        resolve('Operation was successful!');
    } else {
        reject('Operation failed.');
    }
});
```
### Async Await
- `async` keyword declares an asynchronous function.
- When a function is prefixed with async, it automatically returns a promise. If a non-promise value is returned, JavaScript wraps it in a resolved promise.

```javascript 
const fetchData = async () => {
    try {
        const result = await myPromise; // Wait for the promise to resolve
        console.log(result);
    } catch (error) {
        console.error(error); // Handle errors
    }
};

fetchData();
```

```javascript
async function getData() {
    try {
        let response = await fetch('https://jsonplaceholder.typicode.com/posts');
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}

getData();  // This will fetch the data and log it, or log an error if it fails

```
- Errors in asynchronous functions can be caught using `try...catch` blocks
- You can use `await` inside loops to wait for asynchronous operations to complete in each iteration.
- `async` functions always return a promise, even if they don’t explicitly return one.
- `await` can only be used inside `async` functions, and it pauses the execution of the function until the promise is resolved or rejected
	- If the promise is rejected, it throws an error.
```javascript
async function fetchData() {
    const data = await fetch('https://api.example.com/data'); // Waits for the fetch to resolve
    const result = await data.json(); // Waits for the JSON conversion to complete
    return result;
}

```


### `Promise.all()`
- see `Promise.all()` to run asynchronous tasks in parallel and wait for all promises to resolve.
- method that allows you to run multiple promises concurrently and returns a single promise. 
- It resolves when _all_ of the promises in the array have successfully resolved or rejects as soon as any one of the promises rejects.
```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve) => setTimeout(() => resolve(42), 1000));
const promise3 = new Promise((resolve) => setTimeout(() => resolve("Hello"), 2000));

Promise.all([promise1, promise2, promise3])
  .then((values) => {
    console.log(values); // Output after 2 seconds: [3, 42, "Hello"]
  })
  .catch((error) => {
    console.error(error);
  });

```