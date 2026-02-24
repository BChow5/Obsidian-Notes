### hypothesis testing vs Exploratory Data Analysis
- analyst may have a hypothesis to test
- **hypothesis testing** - test hypothesis market share has decreased
	- e.g. has increasing fee-structure led to decreasing market share?
- **Exploratory Data Analysis (EDA)**
	- delving into data
	- examining important interrelations between attributes 
	- identify interesting subsets of observations
	- develop initial idea of possible associations amongst the predictors as well as between predictors and the target variable
	- exploring categorical variables
		- clustered bar chat
		- comparative pie chart
		- cross tabulation table
	- exploring numerical variables
- binning
	- categorizes an attributes numerical (or categorical) values into reduced set of classes
	- makes analysis more convenient
	- e.g. number of Day Minutes could be binned into Low Medium High categories
	- e.g. states may be binned into regions
	- binning is both data preparation and data exploration

### dealing with correlated variables
- should be avoided
- incorrectly emphasizes one or more data inputs
- creates model instability and produces unreliable results
- e.g. day charge and day minutes are correlated
- one of two variables should be eliminated from model
	- day charge arbitrability chosen for removal 
- proceeding to data mining without first eliminating correlated variables may have produced compromised results 