### Summarizing and Presenting One Numerical Variable 
- To summarize a variable numerically, we have two primary types of measure:
	- **measures of center**
		- "middle"
		- what is a typical or central value of X?
		- mean (average), median, mode
	- **measures of spread**
		- "wide"
		- how much does X vary around its center? how spread out is X?
		- range and IQR
		- standard deviation and variance 
```ad-note
We call these measures "statistics" (a numeric summary of the sample) when we are summarizing sample data. They are called **parameter** when we are summairizing population data 

```

![[Pasted image 20240920093318.png]]

### Measures of Center
- the three measures of center we will use are
	- mean
	- median
	- mode

##### Mean
- <mark style="background: #ABF7F7A6;">mean</mark> - the arithmetic mean of a numerical data set is the familiar "average" we are used to
- sensitive to extreme values
	- ![[Pasted image 20240920093443.png]]
	- how to calculate mean:
		- sum of value /(divide) number of values = mean
	- ![[Pasted image 20240920093646.png]]

##### Median
- <mark style="background: #ABF7F7A6;">Median </mark>- the median of a set of numerical values is the middle when the values are sorted in increasing order
- the median is detonated by X~ or Q2
- ![[Pasted image 20240920093924.png]]
![[Pasted image 20240920094407.png]]
- for median, it will also be 50(k) for our calculation
- e.g. 8/2 = 4
	- then you get the average of the values at the 4th and 5th position
- e.g. 7/2 = 3.5 then round to 4

##### Mode
- <mark style="background: #ABF7F7A6;">Mode </mark>(also called the modal value) of a data set is the value that occurs most frequently 
	- if two values of X occur with the same greatest frequency, the data set is said to be <mark style="background: #ABF7F7A6;">Bimodal</mark>
	- if there are more than two modal values, it is said to be <mark style="background: #ABF7F7A6;">Multi-modal</mark>
	- if no value is repeated, we say there is <mark style="background: #ABF7F7A6;">no mode </mark>

#### Sensitivity to Extreme Values
- if a data set contains extreme values, which measure of center best describes the central value?
- generally, the **median** is more representative of the center than the **mean** when there are extreme values since those values "pull up" or "pull down" the mean but don't affect the median 
- for extreme values, use **median** 
- ![[Pasted image 20240920095311.png]]
- if we were to try to calculate the mean, we can assume it would be somewhere to the right of the median since mean is more extreme sensitive
	- the extreme values to the right pull more

### Measures of Spread
- there are four ways that we will measure the spread or variability of a data set
	- **range**
	- **inter-quartile range**
	- **standard deviation**
	- **variance** 
- ![[Pasted image 20240920102449.png]]

#### Range 
-  the range is the distance between the maximum and minimum value
- sensitive to extreme values
- **range = max - min**
- note that range is a single number (because you subtract), not a pair of numbers
- range is simple to computer but it is determined by the extreme values only

#### Inter-Quartile Range (IQR)
-  the distance between the 25th percentile (1st quartile) and the 75th percentile (3rd quartile) of a data set
- **IQR = Q3- Q1**
- Q1 =  total amount * 25/100 = position
	- if decimal, round up. ignore the calculation below
	- e.g. P<sub>25</sub> = 
		* 20 is the 25th position
		* look to the right of it
		* add the two together and divide 2
			* 25th and 26th position
		* 20 + 20/2 = 20
- Q3 = total amount * 75/100
	- if decimal, round up
- IQR is not sensitive to extreme values, but it is not using all of the data points 

#### Standard Deviation
- the standard deviation of a data set is calculated using all of the data points 
- ![[Pasted image 20240920100712.png]]
	- n = sample size
	- N = population size 
	- Xi = the value at the position
	- Xbar = mean of sample
		- mean is the same for everyone one once you've calculated it 
	- mew = mean of population
- ![[Pasted image 20240920100727.png]]
- Use the formula for *s* when you only know *X* for a random sample of the entire population  
- Use the formula for population when you know X for the entire population (which is rare).  
- In either case, the value of 𝐀 or 𝐀 is the typical distance between 𝐀 and the mean value of 𝐀
- ![[Pasted image 20240920104517.png]]\

#### Variance 
- the variance of a data set is directly related to the standard deviation
- specifically, variance in the square of the standard deviation 
- ![[Pasted image 20240920104634.png]]

### Skewness
-  a distribution of data is skewed if it is significantly asymmetrical 
- ![[Pasted image 20240920110159.png]]
- we can quantify skewness with **Pearson's Median Skewness** 
	- ![[Pasted image 20240920110229.png]]
		- xbar = mean
		- xtilda = median
		- s = standard deviation 
	- ![[Pasted image 20240920110239.png]]
- ![[Pasted image 20240920110310.png]]

### Summarizing and Presenting One Categorical Variable: Frequency Tables
- <mark style="background: #ABF7F7A6;">frequency table</mark>
	- counts how many times each value occurs in our sample 
	- to get relative frequencies, divide each frequency by the total size of the sample

##### Example 
- ![[Pasted image 20240925221323.png]]
- the most important way to summarize categorical variables is to calculate proportion
- a <mark style="background: #ABF7F7A6;">proportion </mark>is the fraction of individuals who fall into some specified category
	- we distinguish sample proportion and population proportion
	- x = frequency
	- n = sample or population 
	- ![[Pasted image 20240925222957.png]]
- Both **proportion** and **relative frequency** are calculated using the same formula.
- Proportion is more general, while relative frequency is often used in the context of data from repeated experiments or trials.

### Summarizing and Presenting two Categorical Variable: Two-Way Tables (contingency table)
![[Pasted image 20240925222015.png]]
![[Pasted image 20240925223609.png]]

### Formulas
- **mean**
	- ![[Pasted image 20240925230652.png]]
	- affected by extreme values (outliers)
- **median**
	-  **Arrange the data in ascending order**: Sort the numbers from smallest to largest.
	- **Identify the number of data points (nnn)**: Determine how many numbers are in the dataset.
	- **Determine if the number of data points is odd or even**:
		- If **odd**, the median is the middle number.
		- If **even**, the median is the average of the two middle numbers.
			- if not a whole number, round up
- **standard deviation**
	- - ![[Pasted image 20240920100712.png]]
	- n = sample size
	- N = population size 
	- Xi = the value at the position
	- Xbar = mean of sample
		- mean is the same for everyone one once you've calculated it 
	- mew = mean of population
- **skewness**
	- ![[Pasted image 20240920110229.png]]
		- xbar = mean
		- xtilda = median
		- s = standard deviation 
- **proportions**
	- ![[Pasted image 20240925222957.png]]
	-  x = frequency
	- n = sample or population 
- **variance**
	- ![[Pasted image 20240920104634.png]]
	- specifically, variance in the square of the standard deviation
- **range = max - min**
- **IQR = Q3- Q1**
	- Q1 =  total amount * 25/100 = position
	- Q3 = total amount * 75/100
	- if decimal, round up
	- IQR is not sensitive to extreme values, but it is not using all of the data points 