### Midterm study outline
- Fundamental JS knowledge
	- variables, conditionals, loops, functions 
	- arrays/objects (dictionary)
		- inserting, deleting, iterating for objects (for-in loop)You can use `await` inside loops to wait for asynchronous operations to complete in each iteration.
- Week 1 
	- difference of interpreter vs compiler
		- give an example of each
	- runtime environments 
		- how does it relate to interpreters and compilers 
	- ecmascript
- week 2, week 3
	- synchronous vs asynchronous code
		- explain the difference
	- asynchronous code (callback functions)
- week 3+
	- promises 
		- creating our own promise
		- using the fs/promises functions
		- error handling within promises
		- .then/.catch vs async await
	- error handling in synchronous code


### Fundamental JS Knowledge
- use const be default to make your variables 
	- or use let
	- **DO NOT** use var
		- var keyword makes all variables global 
		- 2 global variables in JS can exist and will overwrite it. so you want const to keep your variables safe 
- loops
	- `for (const element of object) {}`
		- most commonly used
	- `colors.forEach(color => console.log(color))`
- use `===` instead of double equal for comparing
	- == does an implicit type conversion 

#### Arrays and Objects
- `colors.push("yellow")`
	- adds to an array
- `person.hairColor = "black"`
	- insert into an object 
- `Object.keys(person)`
	- gives a list of all the keys in the object
- `Object.values(person)`
	- gives a list of all values in the object
```java
for (const key in person) {
	const value = person[key]
	console.log(key, value)
}
```
- looping through our object 
	- this loop only gives you they key
	- if you want the value, you will need to use the key to get the value 
- only use the `for - of - ` loop for an array. not an object

### Week 1
- programming language
	- JavaScript is ours for the course
- interpreters/compilers are the translator
- programming language and translator compose your runtime environments
- run time environment is how we actually run the code we wrote in our programming language 
	- need the translator still
		- turns our JS code into machine language
	- JavaScript relies on APIs and libraries for additional functions provided in the runtime environment
	- runtime environment will already have the translator inside it
- JS is both interpreted and compiled 
	- v8 engine
		- translates JS into machine code
- JS in the browser isn't exactly the same outside the browser
- Ecmascript is a specification PDF
	- shows which functions are part of JS and will work everywhere
	- not a language
	- tells us what JS as a language is capable of going 
	- only talks about functions purely in JS so anything part of nodeJS is not shown in the ecmascript
		- i.e. node module we import aren't in there

### Week 2 and 3
- technically any function can have a callback function but that doesn't make it asynchronous 
- if a function is doing something with the fs module, it can be asynchronous 
	- check that it isn't using the sync version of the fs functions 
- without sync, we need to add a callback function
	- it returns the data into our callback 
- T or F if you have 2 asynch operations, should they always be nested?
	- F - we don't always need to nest them
	- only nest when the second operation depends on the first one 
- 2 fundamental problems for asynch callbacks
	- readability issue
	- no centralized error handling 
	- this is why we moved to promises

### Week 3+
- don't need to write our own promises but need to recognize that code
	- 2 MCQ on it 
- how we promisify a function that doesn't work with promises
```java
const readFileP = (fileName) => {
	return new Promise ((resolve, reject) => {
	fs.readFile(filename, (err,data) => {
		if (err) {
			reject(err)
			} else {
				resolve(data)
			}
		})
	})
}
```
- doing this lets us either resolve or reject the data we get from this function
- while your system is still doing the function, 
	- the promise status is pending
		- can either be:
			- fulfilled
			- rejected
			- pending 
	- the promise result is undefined
- when you need to call a function but that function doesn't return a promise for your next step:
	- `Promise.resolve()`
		- this makes an empty promise chain
		- we can use .then for our function after now 
- async await
	- is an alternative to saying .then and .catch
	- just adding the word await to our function call
		- then we can save it to the variable
	- need to add async to our function call to let it know you're using async await 

