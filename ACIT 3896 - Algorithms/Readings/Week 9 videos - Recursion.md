### Intro to recursion 
- recursion is when a function or method calls itself as if it were a helper method/function for itself
	- if careless, you can cause an infinite loops

#### key plan
- one or more smaller problems
- figure out how to stop

**for recursion, we need**
- 1 or more base cases
	- so easy its trial
	- don't recurse (can't call yourself)
	- a foundation, a base
- 1 or more recursive cases
	- do call yourself
	- BUT you need to get closer to the base case 
		- so that you eventually reach the base case to stop

![[Pasted image 20250316211029.png]]
- the base case:
	- when the function does not call on itself

![[Pasted image 20250316211645.png]]

### Recursion and DFS
![[Pasted image 20250316212948.png]]
- we still have a stack here but we're not managing it 
	- its not explicitly written out stack like in previous code 
![[Pasted image 20250316213009.png]]

#### recursive DFS sum
![[Pasted image 20250316213510.png]]

#### callback
![[Pasted image 20250316213747.png]]