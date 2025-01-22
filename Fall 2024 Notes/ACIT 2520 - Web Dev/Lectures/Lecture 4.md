#### Two fundamental problems on async code in Node
#midterm 
1. no centralized error handing (try and catch won't work for async code)
2. readability issue (hard to follow along with/read the code)
	- callback hell issue 
- Solution: Promise

### Promises
- promises are basically an object (dictionary)
- the values in the object change 
- always has a few things in it:
	- status:
		- pending (default status)
		- fulfilled 
			- operation was completed successfully
		- rejected
			- operation failed 
	- result 
		- undefined (default result)

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

#### Special calls for promises
- .then()
- .catch()