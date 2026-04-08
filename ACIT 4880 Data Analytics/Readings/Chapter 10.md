### Clustering task
- euclidean distance measures distance between records
- other distance measures are city-block distance and minkowski distance
- "different from" function measures similarity between categorical attribute
	- ![[Pasted image 20260408105518.png]]
- normalizing enhances performance of clustering algorithms
- algorithms construct clusters where between-cluster variation (BCV) large as compared to within-cluster variation (WCV)

### hierarchical clustering
- clustering algorithms are either hierarchical or non 
- **divisive methods**
	- all records initialized to single cluster
	- each iteration, most dissimilar record split off into separate cluster
	- continues until each record represents a single cluster 
	- It keeps splitting the clusters into smaller and smaller pieces until every data point is back to being alone.
- **agglomerative methods**
	- most common approach
	- every data point starts as its own individual cluster
	- algorithm looks for two points that are most similar and merges them into a pair then repeat
	- continues until every point has been merged into 1 giant cluster 

### distance between clusters 
- **single linkage (nearest neighbor)**
	- looks at shortest possible bridge between two points
	- find 1 point in cluster A then 1 point in cluster B that are the closest to each other
		- that gap is the distance for the whole group
	- result: creates long snake like clusters
	- downside: suffers from chaining. because it only cares about the closest neighbors, two very different groups can be merged because they happen to have 2 points sitting near each other
	- can be heterogenous (mixing things that don't belong together)
- **complete linkage (farther neighbor)**
	- conservative approach
	- won't merge 2 groups unless even their most distance members are relatively close 
	- the maximum distance defines the groups
	- result: forces clusters to be compact and spherical
	- downside: very sensitive to outliers. 1 stray data point far out in the woods will make the whole cluster seem much farther away than it actually is
- **average linkage (middle ground)**
	- democratic approach
	- calculates distance between every possible pair of points then takes the average of all those distances
	- result: much more stable and less influenced by single extremes or bridges of other two methods

### k-means clustering
- goal is to partition your data into k distinct non overlapping groups (clusters)
- how it's done:
	- choose k (how many clusters you want)
	- picks k random points in your data to act as "centroids" (center of cluster)
	- every data point looks at the centroids and joins the one it's closest to
	- after groups are formed, algorithm calculates the mean of all points in each group. the centroid then moves to that new average position
	- repeat step 3 and 4 until centroids stop moving 