### A comment on order
- for a given node, what order should we do the children?
	- usually there is some defined order
- does the parent come before the child?
	- we don't have to process the parent first in the answer 
	- we do need to visit the parent to find the children though 

#### pre-order
- A -> B -> C -> D
![[Pasted image 20250303165526.png]]

#### post-order
-  B -> C -> D -> A
- its the order of our processing 
- parent vs child visit is in the opposite order 

#### in order
- left branch first, then parent, then right branch

### Queues
- FIFO (first in, first out)
- oldest element is removed first 
![[Pasted image 20250303170934.png]]

#### just using a list
![[Pasted image 20250303171802.png]]
- slower performance speeds on remove

#### with linked list
- better performance for remove no matter where it is 
- more difficult to set up right
![[Pasted image 20250303171817.png]]
![[Pasted image 20250303171743.png]]

#### deque
- just know its a thing, will talk more in class

### BFS
- Breadth First Search
- so go to the next one on the same level before we go deeper
![[Pasted image 20250303172727.png]]
- BFS uses a queue
- we uses FIFO to get the breadth search rather than depth

### DFS vs BFS 

#### Execution time (asymptotic)
- both run in linear time
	- asymptotically equivalent 

#### memory usage (asymptotic)
- DFS: how big can the stack be?
	- limited by the current depth 
		- less than or equals to the tree's height
	- probably roughly logarithmic 
- BFS: how big can the queue be?
	- limited by how many nodes can you discover and not be finished processing?
	- limited by the last layer 
	- often this is half the entire tree or more (but not necessarily)
	- often linear 
- often, but not always. win for DFS

#### behaviour on infinite trees
- like trees of imaginary ideas, not trees of data
- they expand as you search them
- DFS on a tree with no bottom will not work out
- 
