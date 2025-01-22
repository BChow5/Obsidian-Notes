### Performance
- there are different types of performance improvement
	- parallelization is can speed up stuff a lot
- scale dependent performance is what we care about for algorithms
	- e.g. linear search gets slower for bigger lists but is fine for small lists

### Measure

#### How do we measure?
- measure the current time
- run the race
- measure the current time
- subtract 
- use `time.perf_counter_ns()` for our time to complete test
	- good for our testing but note that it isn't relative to anything else
	- don't use it for anything else 
- you will want to test the time multiple times because you could have variables that affect time
	- e.g. you get lucky and the number you're looking for is early on so you finish it faster
	- you should get the average time from the tests
- make sure you don't count the setup time to get your dataset into your testing
	- you only want to test the thing you want. not the setup
- we care about how the time changes as the size of the input changes 
