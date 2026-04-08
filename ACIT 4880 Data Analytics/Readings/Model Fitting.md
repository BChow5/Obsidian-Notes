### model complexity and fit
- underfitting - model is too simple to capture underlying pattern of the data
	- training and test error will be high
- overfitting - model is too complex and memorizes the noise in the training data
	- low training error but high test error
- balanced fit - ideal state where the model captures the true pattern and performs well on training and test data

### bias variance tradeoff
- bias - error caused by simplistic assumptions
	- high bias leads to underfitting
- variance - error caused by model being too sensitive to small fluctuations in the training
	- high variance leads to overfitting
- want to find the sweet spot that balances the 2

### parameters vs hyperparameters
- model parameters - internal values learned from training data
- hyper parameters - external configurations that control the learning process

### SVM tuning
- hyperplane - the decision boundary that maximizes the margin between different classification groups
- kernel - transformation used to separate data that is not linearly separable in its original dimension
	- common types include linear, poly, rbf
- gamma
	- high gamma - model considers only points close to the boundary (can lead to overfitting)
	- low gamma - model considered points further away to create a smoother boundary
- regularization (C): 
	- high C - prioritizes classifying training points correctly (narrower margin, higher risk of overfitting)
	- low C - prioritizes larger margin even if it misclassifies some training points (higher bias, lower variance)

### hyperparameter optimization methods
- GridSearchCV tries every possible combination of hyperparameters in a specifified grid
	- exhaustive and guaranteed to find the best combination in the grid
	- slow for large datasets
- RandomSearchCV
	- picks combinations at random from the grid
	- faster than gridsearchCV and often finds a good enough solution gaster
- 5-fold cross validation
	- technique used with these searches where the data is split into v5 parts
	- model trains of 4 and tests of 5
	- repeats this 5 times to get an average score
	- 