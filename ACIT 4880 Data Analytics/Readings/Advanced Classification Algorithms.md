### Support Vector Machine
- goal of SVM is to find the boundary line that best separates different classes
- it maximizes the distance between the line and the closest data point
	- known as support vectors
- sometimes data isn't separable by a straight line
	- SVM uses "kernels" to project data into a higher dimensions where it can be separated linearly
- best for high dimensional data (lots of features) and cases where there is a clear magic of separation

### Decision Tree
- splits data into branches based on feature values
- splitting criteria: it decides how to split the data using metrics like Gini impurity or Information Gain (entropy) 
	- wants to make each leaf as pure (one single class) as possible
- downside: prone to overfitting
	- can become so specific to your training data that they fail to predict new data accurately
- best for: simple interpretations 
	- easy to explain to non technical people why a decision was made

### Random Forest
- ensemble method that builds many trees and merges them together
- bragging (bootstrap aggregating): creates multiple trees using different random subsets of the data and different random subsets of features
- voting: for a final prediction, every tree in the forest gets a vote
	- class with the more votes wins
- benefit: drastically reduces overfitting
	- even if one is off, average of 100 trees will likely be more accurate
- general purpose classification where accuracy is more important than seeing the exact why behind every single split

| **Feature**             | **SVM**                 | **Decision Tree**  | **Random Forest**   |
| ----------------------- | ----------------------- | ------------------ | ------------------- |
| **Interpretability**    | Low (Hard to visualize) | High (Very visual) | Medium (Complex)    |
| **Risk of Overfitting** | Low (if tuned)          | Very High          | Low                 |
| **Computation Speed**   | Slow on large datasets  | Fast               | Slower (more trees) |
| **Handle Outliers**     | Sensitive               | Robust             | Very Robust         |