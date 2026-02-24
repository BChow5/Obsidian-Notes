- trees are organizing data in a hierarchical structure 

### Why should we care?
- good for modeling real life data
	- examples
		- reporting hierarchy at work
		- tree of life
- a component of more sophisticated algorithms/data structures 
	- algorithms can get performance benefits from using trees
	- examples
		- compression (zip, lzh, etc.)
		- geometry (k-d trees, BSP trees, etc.)
		- self balancing binary trees
		- heap
- many software tools/libraries/API use trees for interface
	- examples
		- DOM
		- filesystem
		- programming language syntax
		- JSON/serialization

### what is a tree?
- tree is a container like lists or dictionaries
- Lists
	- structured and ordered
- Dictionary
	- not structured or ordered 
- Linked lists
	- structured
	- ordered
	- next and previous

#### Trees
- have nodes (aka vertices/vertex for single)
- edges
	- lines connecting the nodes 
- structured but no first and last 
- hierarchy 
- 1 special node is called the **root** (drawn at the top)
- no cycles 
- trees have a **parent** and **child** relationship
	- looking at a pair of vertices, the node closer to the root is the parent 
- every node has 1 parent
	- except the root
- nodes can have any number of children
	- including 0 children 
- nodes with no children are called **leaves** 
- **ancestor**: parent, parent's parent, etc.
	- sometimes ancestor is defined to include itself 
		- will be mentioned if that's the case
- **descendant**: child, child's child, etc.
	- sometimes descendant is defined to include itself 
- **size**: number of nodes
- **distance** (between two nodes): number of edges to get to each other 
- **depth** (of node): distance of node to root 
	- root always has depth 0 
- **height** (of node): distance of node to furthest descendant 
- **depth of tree**: maximum depth of any node
- **height of tree**: max height of any node 

```python
class TreeNode:
	__init__(self, contents)
		self.contents = contents
		self.parent = None
		self.children = []
```

### alternative to trees 
- graph
	- not like the normal graph we think of 
	- 