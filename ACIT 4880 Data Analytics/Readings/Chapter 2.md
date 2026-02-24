### why process data?
- raw data is often unprocessed, incomplete, noisy
- may contain
	- obsolete/redundant fields
	- missing values
	- outliers
	- data in form not suitable for data mining
	- values not consistent with policy or common sense
- for data mining, database values should undergo data cleaning and transformation
- minimize GIGO
	- garbage in, garbage out 

### handling missing data
- missing values pose problems to data analytics methods
- more common in databases containing large number of fields
- absence of information rarely beneficial to task of analysis
- in contrast, all things being equal, have more data almost always better
- do you delete records with missing values?
	- that is a dangerous pattern because missing values may be systematic
	- valuable info in other fields would be lost 
- data imputation methods are good for this
- there are 3 alterative methods that aren't entirely satisfactory 

#### 1. replacing missing values with constant
- e.g. missing values replaced with 0.0 or Missing

#### 2. replace missing values with mean or mode
- replace with mode or mean sometimes works but end users need to be informed
- mean not always best choice for "typical" value
- resulting confidence levels for statistical interference become overoptimistic since measures of spread are artificially reduced 
- benefits and drawbacks resulting from replacement must be carefully evaluated against possible invalidity of results 

#### 3. replace with random values
- benefit: measures of location and spread can remain closer to original
- but no guarantee the result records make sense 

#### data imputation methods
- what is the likely value, given record's other attribute values?
- requires tools like multiple regression, or classification and regression trees 

### identify misclassifications
- check classification labels to verify values valid and consistent 

### graphical method for identifying outliers
- outliers are extreme values that go against the trend of remaining data
- outliers may represent errors in data entry
- even if valid data, some statistical methods are sensitive to outliers and may produce unstable results
- two graphical methods presented

#### 1. histogram
- examines values of numeric fields

#### 2. 2d scatter plot
- two dimensional scatter plot helps to determine outliers in more than one variable

### measures of center and spread
- estimate where the center of a particular variable lies 

#### measures of center
- most common measures of center
	- mean, median, mode
	- there is a special case of measures of location which indicate where a numeric variable lies (e.g. percentile and quantiles)
- mean
	- average of the valid values of a variable
	- mean is not always ideal
		- on extremely skewed datasets, it is less representative of variable center
		- sensitive to outliers
- median
	- field of value in the middle when field values are sorted in ascending order
- mode
	- field value occurring with the greatest frequency
		- pros: can be used with numerical or categorical data
		- con: not always associated with the variable center  
- measures of spread do not always occur 

#### measures of spread
- measures of location not enough to summarize a variable
- measures of center do not provide a complete picture
- measures of spread of measures of variability complete the picture by describing how spread the fata values of each portfolio are
- typical measures of variability
	- range
	- standard deviation
	- Mean Absolute Deviation
	- interquartile range

### data transformation
- variables tend to have ranges different from each other
- some data mining algorithms adversely affected by difference in variable ranges
- variables with greater range tend to have larger influence on data model's results
- numeric values should be normalized
- standardizing scales the effect each variable has on results 
- neural networks and other algorithms that make use of distance measures benefit from normalization 

### min-max normalization
- determines how much greater field value is than minimum value for field
![[Pasted image 20260204123724.png]]

### z-score standardization
- widely used in statistical analysis
- takes difference between field value and field value mean
- scales this different by field value standard deviation
![[Pasted image 20260204124012.png]]

### decimal scaling
- ensures normalized values lie between -1 and 1 
![[Pasted image 20260204124049.png]]

### flag variables
- some numerical methods require predictors to be numeric
- flag variables (dummy or indicator variables) is a categorical variable with only two values
- define k-1 variables for the flags
	- one of them will just be the default if all flags are 0

#### transforming categorical variables into numerical variables
- why not transform categorical variable region into a single numerical?
	- e.g. north =1 south = 2, etc
- this is a common hazard error
	- this algorithm would then assume that the regions are ordered
	- west > south> east> north
	- west is three times closer to south than north etc 
- should be avoided except with categorical variables that are clearly ordered

### binning numerical variables


### Mean Absolute Error MAE
- average absolute difference between predicted values and actual values
- same unit as target variable
- treats all errors equally
- less sensitive to outliers

### Mean Squared Error MSE
- average of squared errors
- large errors get squared so they matter a lot
- very sensitive to outliers
- commonly used for model optimization

### Root Mean Squared Error RMSE
- the square root of MSE
- RMSE keeps the outlier penalty of MSE but makes the result interpretable 
- still sensitive ti outliers
- easier to interpret than MSE

### R^2
- measures how much of the variance in the target variable is explained by the model
- 1.0 = perfect model
- 0 = no better than predicting the mean
- <0 = worse than predicting the mean

### k-Nearest Neighbors (k-NN) algorithm
- supervised learning algorithm for classification and regression
- smaller k = overfitting
- larger k = underfitting
- 

