### supervised vs unsupervised
- data mining is supervised or unsupervised
	- unsupervised methods means there is no specific target variable
- most common unsupervised method is clustering
- most data mining methods are supervised
	- has pre-specified target variable
	- algorithm is given examples where the value of the target value is provided
		- learns which values of the target variable are associated to which values of the predictor variables
	- regression is a supervised method
	- all classification methods (decision tree, nearest network, k-nearest neighbours) are supervised
- unsupervised does not mean that they require no human involvement 

### cross-validation
- data mining can return phantom results due to random variation
- cross validation ensures that results covered in an analysis are generalizable to an independent unseen data set
- most common methods are twofold cross validation and k-fold cross validation
- splitting data into training set and test set 
- build data mining model using the set data
- evaluate the data mining model using the test data set

### overfitting
- usually accuracy of model is not as high on the test set as it is on training set
- overfitting happens when model tries to fit every possible trend/structure in the training set
- need to balance the model complexity (resulting in high accuracy training set) and generalizability to the test/validation sets
- as model complexity increases, error in training set falls and the test set starts to increase
	- this caused by model becoming memorized training set rather than leaving room for generalization to unseen data
- optimal model complexity is the point where minimal error rate on the test is located 

### bias variance trade off
- bias - errors caused by over simple assumptions
	- model misses the relevant relations between features and target
	- underfitting: the model is too dumb to see the pattern
- variance - errors caused by extreme sensitivity to fluctuations in the training data
	- overfitting: the model "memorizes" the noise instead of the pattern
- as you increase the complexity of your model (e.g. more branches to a decision tree)
	- bias decreases: the model becomes flexible enough to represent the true pattern
	- variance increases: the model starts to follow the random noise in your specific data set
- want to find the sweet spot where Total Error is at its lowest
- we want low bias and low variance
	- low bias, high variance - overfitting
	- high vias, low variance - underfitting 
	- high bias, high variance - worst case

### balancing the training data
- balancing is recommended on classification models when one target class has much lower frequency than the other classes
- two ways of balancing
	- resample the number of fraudulent records
	- set aside a number of non fraudulent records 
	-   

### establish baseline performance
- need a baseline performance to know if our results are good
- type of baseline depends on the way the results are reported
- in an estimation task using regression, our baseline may take the form of a Y model
	- the model simply finds the mean of the response (target) variable, and predicts that value for every record 
	- 