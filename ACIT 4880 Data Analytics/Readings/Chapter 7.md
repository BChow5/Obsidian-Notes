### classification task
- classification is probably the most common data mining task
- e.g. medicine - diagnose whether a disease is present
- in classification, there is a categorical target variable partitioned into two or more classes
	- e.g. low, middle, high for income
- algorithm examines relationships between the values of the predictor fields and the target values

### k-nearest neighbour algorithm
- example of instanced based learning where training set records are first stored
- next the classification of a new unclassified record is performed by comparing it to records in the training set most similar
- used most often for classification
- if k is too small, then its very sensitive to noise
- if k is too big, then it looks too far for neighbours

### distance function
- mathematical way to measure how far apart or different 2 data points are
- euclidean distance
	- straight line between two points in a multi dimensional space
- manhattan distance
	- cant walk through directly, need to fllow streets
	- sums the absolute differences of their coordinates
- we use normalization because one or more ateibutes can have very large values, relative to the other attributes
- for categorical attributes, we use "different" instead of euclidean distance
- could be normalized using min-max,z-score
- different normalization techniques resulted in different results for our training set
- important to understand which technique is being used

### combination function
- decides who has the most influence of the neighbours
- simple unweighted voting
	- every neighbour has 1 equal vote regardless of how close they are
	- prone to ties and can't tell the difference between 2 neighbours that could be far and 1 close
- weighted voting
	- closer neighbours are more trustworthy and have a larger vote
	- weight of the vote is inverse proportional to the distance

### quantifying attribute relevance: stretches the axes
- if an attribute is useless for prediction, KNN will still use it to calculate distance 
	- important variables might be pushed away mathematically because its a different value for an unimportant variable
- noise can cause the model too pick the wrong neighbours
- solution is to stretch the axes
	- way to manually (or automatically) tell the algorithm to pay more attention to specific columns
	- you can multiple each attribute by a weight or coefficient
	- high coefficient stretches the axes
		- differences in this variable now result in larger distance to make the model more sensitive to the attribute
		- low coefficient shrinks the axes for less impact 
- how to find the right weights?
- domain expertise and cross validation 
- 