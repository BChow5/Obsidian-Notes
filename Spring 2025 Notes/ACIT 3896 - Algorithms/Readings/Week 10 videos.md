### Sorting Howto and Philosophy

#### 2 ways to sort in python
- sorted()
	- returns a new sorted list
- .sort 
	- doesn't return anything
	- changes the original list 
- these can sort float and ints
- can sort strings
- can't sort strings and numbers together
- can sort lists of lists of numbers
- can't sort list dictionaries
- you can customize sorted
	- e.g. sorting strings by length instead of alphabetical 
![[Pasted image 20250317173109.png]]
![[Pasted image 20250317173228.png]]

#### recap
- python sorting is flexible
- python built in sorting is world class and we won't write a better one generally
- why learn sorting?
	- everyone expects you to 
	- trains our minds
		- HOW does it work?
- our sort functions in this class will be functions but will work by mutating (not returning)

### our first official sort
#### bubble sort
- big things will bubble up first
![[Pasted image 20250317175330.png]]
![[Pasted image 20250317175638.png]]
- reducing that amount of comparisons we do because we know the end is already sorted 
- little bit faster 

![[Pasted image 20250317180107.png]]
- keeps a check to see if we did any swap
- if no swaps made, then that means the list is already in order and we can end early
- might not always be faster but it could be 


### Big-Oh Polynomial Pseudocode
- linear: O(n)
	- "big O of n"
	- "order of n"
- technically big O can apply to all sorts of things like memory
	- practically, it usually always refers to speed 
- why would code a cost time of O(n)?
	- usually because it needs to visit every bit of data (at least) once
![[Pasted image 20250317182654.png]]

- in big O, we never worry about the coefficient 

![[Pasted image 20250317182954.png]]


![[Pasted image 20250317183044.png]]
- nested for loop is the most common way for quadratic 
	- could go deeper to cubic like this too 

![[Pasted image 20250317183243.png]]