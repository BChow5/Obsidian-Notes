### Stacks
- stack is a data structure 
	- inferior to python list
- contains data 
- inserting data is "putting it to the top of the stack"
- we use "push" to insert data
- we use "pop" to take out
	- cant specify an index
- last in, first out 

#### How to implement?
- should just use a list
- wrapper class but still uses a list 
- use a linked list 
	- do push and pop at the front of the list 

#### Why do we care?
- predictability of the call stack 
- found in other programmer stuff

### DFS
- Depth First Traversal 
- e.g. what do we do if we want to visit every node in a tree?

- order going down the tree
	- start at root
	- any time you see a node with unvisited children, visit leftmost unvisited child
	- otherwise go to parent 
- also called Depth First Search 
- "process" each node only once 
	- process when you first arrive 

### "mouse in the maze" DFS algorithm 
- visit the node
- make a record of the visit in the stack
	- also track how many children have been processed 
- if the number of children matches the amount of children they've solved, then you pop it 

e.g.
- get to the top of the stack (pop)
- look at this record
- if the second number is less than the number of children
	- increase the second number
	- put the record back (with its new larger number)
	- add a new record
- otherwise:
	- don't

### "The todo list" DFS algorithm
- every time to visit a node:
	- pop a node
	- process it
	- put all its children on the stack