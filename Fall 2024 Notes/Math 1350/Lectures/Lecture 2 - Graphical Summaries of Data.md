#### Numerical Variable Graphs
- frequency table
- histogram
- percentile
- quartile
- five number summary
- boxplot

#### Categorical Data
- Bar chart
- Pie chart

### Frequency Distribution


- <mark style="background: #ABF7F7A6;">Cumulative Frequency </mark>
	- adding up all the frequencies up to that point (inclusive)
	- e.g. 22 + 31 + 18 = 71 is the Cumulative Frequency
- <mark style="background: #ABF7F7A6;">Relative Frequency </mark>
	- calculating proportions
	- a measure used in statistics to show how often an event occurs relative to the total number of events or observations (proportional) 
	- ![[Pasted image 20240913100028.png]]
- <mark style="background: #ABF7F7A6;">Cumulative Relative Frequency </mark>
	- sum up all the relative frequency at and below our desired value 
	- e.g. 0.31 + 0.22 
	- we are adding the relative frequencies 
- <mark style="background: #ABF7F7A6;">Modal Class</mark>
	- the most frequently occurring class
- <mark style="background: #ABF7F7A6;">Class Mark</mark> (also called the **midpoint** or **class midpoint**)
	- the value in the middle of a class interval in a frequency distribution. 
		- It represents the average value of a given class interval and is used to summarize the data.
	- midpoint of the limit
	- ![[Pasted image 20240913093849.png]]
- <mark style="background: #ABF7F7A6;">Class Boundary</mark>
	- the actual limits of a class interval in a frequency distribution. It helps eliminate gaps between class intervals and ensures that all data points are accounted for.
	- **Lower Class Boundary** - middle of the lower limit of current class and upper limit between previous class 
	- **Upper Class Boundary** - middle upper limit of our current class and lower limit of next class
- <mark style="background: #ABF7F7A6;">Class Width</mark> (class interval width)
	- the difference between the upper and lower boundaries (or limits) of a class interval in a frequency distribution
- <mark style="background: #ABF7F7A6;">Level of Precision</mark> 
	- the specificity of the unit of measure that the class data will be measured in
	- the measurement for how accurately we are talking about the data 
	- e.g. every one year when we're talking about age
		- no 20.5. only 20,21,etc. 
		- if we wanted to increase the level of precision we can add the decimal

##### Example
![[Pasted image 20240913093408.png]]
Using the sample data for X = Age given above, we obtain the frequency distribution below
![[Pasted image 20240913092650.png]]

- What are the class limits of the modal class?
	- lower class limit = 20
	- upper class limit = 24
- what is the class mark of the modal class ?
	- 20+24/2 = 22
- at what level of precision are we measuring X?
	- one year old
		- means we don't use have people at age 20.5
- what are the class boundaries of class 20-24?
	- lower class boundary = 19.5
	- upper class boundary = 24.5
- what is the class width for this frequency distribution? 
	- 24.5 - 19.5 = 5

### Histograms 
-  a <mark style="background: #ABF7F7A6;">Histogram</mark> is a visual representation of the corresponding frequency distribution
	- you need to determine class boundaries to correctly position the left and right side of the rectangles 

```ad-example
title: Histogram Example
![[Pasted image 20240913094647.png]]
![[Pasted image 20240913100739.png]]

```
- ![[Pasted image 20240913094731.png]]

### Terminology Recap
- <mark style="background: #ABF7F7A6;">Lower Class Limit</mark>
	- the smallest value of X in a class (based on the level of precision)
- <mark style="background: #ABF7F7A6;">Upper Class Limit</mark>
	- the largest value of X in a class (based on the level of precisions)
- <mark style="background: #ABF7F7A6;">Class Mark of a class</mark>
	- the average of its lower class limit and its upper class limit
- <mark style="background: #ABF7F7A6;">Class Boundaries</mark>
	- the numbers in between the upper class limit for the chose class and the lower class limit of the next class
- <mark style="background: #ABF7F7A6;">Class Width</mark>
	- is the distance from one class boundary to the next class boundary 

##### Histogram recap
- each rectangle goes from one class boundary to the next class boundary
- there must be no gaps between the rectangles
- the classes should be of equal width and must cover all observed values of X
- the number of classes should be approximately ![[Pasted image 20240913101435.png]] $\sqrt[3]$



### Percentiles and Quartiles 
- the K<sup>th</sup> **percentile** of a numerical variable X is denoted P<sub>k</sub>
	- it divides the data into two parts:
		- the lower - k% of values 
		- the upper - (100-k)% of values
	- the lower k% of values have to be less than P<sub>k</sub>
- <mark style="background: #ABF7F7A6;">Percentile </mark>
	- a value below which a given percentage of observations in a data set falls. 
	- e.g. the 25th percentile is the value below which 25% of the data points are found. 
	- Percentiles are useful for understanding the relative standing of a value within a distribution.
- <mark style="background: #ABF7F7A6;">Quartiles</mark> are **percentiles** in multiples of 25

##### Example 1
![[Pasted image 20240913103404.png]]

Determine:
P<sub>25</sub> = 
* 20 is the 25th position
* look to the right of it
* add the two together and divide 2
	* 25th and 26th position
* 20 + 20/2 = 20

P<sub>75</sub> = 
* 31 is the 75th position
* look to the right of it 
	* 75th and 76 position
* 31 +32/2 = 31.5

P50
- 25 is the 50th position
- look to the left of it
	- 75th and 76 position
- 24 +24/2 = 24

<mark style="background: #ABF7F7A6;">Quartiles</mark> are percentiles in multiples of 25
![[Pasted image 20240913103941.png]]
![[Pasted image 20240913104032.png]]

if X is a numerical value, then the following five statistics make up the <mark style="background: #ABF7F7A6;">Five Number Summary </mark>

<mark style="background: #ABF7F7A6;">Five Number Summary</mark> is a set of descriptive statistics that provides a concise overview of the distribution of a data set. It includes five key values:
- need to make the numbers in ascending order
1. **Minimum**: The smallest value in the data set.
2. **First Quartile (Q1)**: The 25th percentile, where 25% of the data falls below this value.
3. **Median (Q2)**: The middle value, or the 50th percentile, where 50% of the data is below this value and 50% is above it.
4. **Third Quartile (Q3)**: The 75th percentile, where 75% of the data falls below this value.
5. **Maximum**: The largest value in the data set.

##### Example 2
On the Math 12 Provincial Exam, Bill scored in the 75th percentile. How do we interoperate this?
- **Correct:** Bill performed better than 75% of people
	- INCORRECT: Bill score 75 points
- **Percentile**: A percentile rank indicates the percentage of scores that fall below a particular value.
	- desired percentile multiplied by number of data points = position of the percentile (need to round up)
	- e.g. Bobby is in the 30 percentile of a set of 25 students
		- 0.3* 25 = 7.5 = 8
		- so above the 8th position is being in the 30th percentile 
- **75th Percentile**: This is the value below which 75% of the scores in the data set fall. It also means that 25% of the scores are above this value.

### How to Calculate Percentiles by Hand
![[Pasted image 20240913110335.png]]
### Box plots
![[Pasted image 20240913110402.png]]
* anything outside the min or max value are outliers 
* <mark style="background: #ABF7F7A6;">interquartile range</mark> is the range of the middle 50% of the observation (the box)
### Outliers
- <mark style="background: #ABF7F7A6;">Outliers</mark> are extremely large or small values
	- should we delete outliers?
		- if it is a mistake, then we should delete. only if the measurement is incorrect
		- if not a mistake, we should keep
- we can never be certain when an extreme value is due to a mistake or not
	- we need to make an objective rule to identify possible outliers so the decision does not come down to subjective preference 

##### How do we calculate outlier?
- the rule we will use for identifying outliers is based on the Inter Quartile Range (the box)
- ![[Pasted image 20240913110831.png]]
	- this is done using the values at these points
- any value or above the upper limit are marked as outliers 
- ![[Pasted image 20240913111111.png]]
- ![[Pasted image 20240913111102.png]]

### Summarizing and Presenting One Categorical Variable

First let's consider one specific categorical variable X = eye colour (from data2.slsx)

#### Bar Chart
![[Pasted image 20240913111234.png]]
- the possible values of X go along the horizontal axis
- the frequencies (count) go along the vertical access 
- a bar chart is like a categorical version of a histogram 

#### Pie Chart
![[Pasted image 20240913111355.png]]