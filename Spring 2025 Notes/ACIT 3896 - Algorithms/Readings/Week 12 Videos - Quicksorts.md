### Quicksort
- divide and conquer algorithm
- how it works
	- split the input
	- conquers problems individually
	- takes the solutions and combines them to a full solution
- merge sort does something simple for the split and does work in merge
- quicksort does most work in the split and easy combine 
![[Pasted image 20250331194026.png]]
- because our combine is simple, our split must be smart
	- it must make big numbers end up on the right and small to the left 

#### how to:
1. split the input in half (partition) without sorting it, but we need small stuff on left and big on right
2. "conquer" it (recurse)
3. combine by simple concatenation 

#### requirements for partition 
1. split the list into two small lists with small stuff on left and big on right
2. split roughly in half 
3. do it in linear time 

- we will have a number called "the pivot"
- write a helper function called partition that makes the left and right lists with the right things
	- returns them
![[Pasted image 20250331195117.png]]
- don't need to pick a perfect pivot otherwise its too slow
- so we can pick 2 random ones, 1 middle and then get the median
	- not perfect but its good enough
- sometimes people just pick the 1 at the end to be the pivot (good enough)
![[Pasted image 20250331201249.png]]

### Quicksort in place
