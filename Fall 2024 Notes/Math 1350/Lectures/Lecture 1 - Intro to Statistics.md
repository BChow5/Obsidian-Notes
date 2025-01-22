### What is Statistical Data?
* stats deals with variables (x, y, z, etc.) that do not necessarily have a definite value
* a variable may have a different value each time we measure or observe it

```ad-example
title: Variables Example
- this spreadsheet contains observations of several variables for a set of students
- ![[Pasted image 20240906084753.png]]

```
* the term "Individual" can refer to people or objects 

#### Definitions
- <mark style="background: #ABF7F7A6;">Population </mark>- the complete set of individuals (people, objects, etc.) we want to know about
- <mark style="background: #ABF7F7A6;">Sample</mark> - a subset of individuals selected from a population
- <mark style="background: #ABF7F7A6;">Sampling Frame </mark>- a physical list of all the individuals in a population
- <mark style="background: #ABF7F7A6;">Variable</mark> - X is any number or characteristic that we could measure for each individual in the population
- <mark style="background: #ABF7F7A6;">Observation</mark> or <mark style="background: #ABF7F7A6;">Measurement</mark> of X is the value of X for one specific individual
	- e.g. The age of an individual is an observation of the variable Age
- a <mark style="background: #ABF7F7A6;">Data Set</mark> is a set of measurements of certain variables for individuals in a certain sample (or the entire population)

```ad-example
title: Population vs Sampling Frame Example
- Population = all the people who work in Canada
	- Where do we get that list?
- Sampling frame = we get a list of residents in Canada
	- Some people may not be working in this list
	- So this list may not match what we are originally looking for
- Population can be more theoretical for what we want vs Sample Frame is the actual list we have

```

### Method of Sampling 
#### Random Sampling (also called Probability sampling)

![[Pasted image 20240906090029.png]]
- <mark style="background: #ABF7F7A6;">Simple Random Sample</mark>
		- e.g. pulling people's names out of a box
- <mark style="background: #ABF7F7A6;">Systematic Sample</mark>
	- 3 steps:
		- 1. put people on a sequential list 
		- 2. pick first individual to start our sample
			- first individual picked in a random way
		- 3. continue with the step size
			- e.g. every 3rd person 
			- but how do we determine step size?
			- population size divided by our desired sample size 
- <mark style="background: #ABF7F7A6;">Stratified Sample</mark>
	- referring to groups 
	 - 2 steps
		 - 1. homogenous group
			 - people in selected group share some kind of common feature(s)
		- 2. select individuals to make sample
			- selected from each group in a proportional way 
	- e.g. 1: polling high school students and wanting students representing each category of grade 8,9,10,11,12
	- e.g. 2: company polling people based on job titles like manager and non managers 
-  <mark style="background: #ABF7F7A6;">Cluster Sample </mark>
	- referring to groups 
					2 steps:
						1. makes (random) groups but they don't need to be homogenous
						2. select groups to make sample 
					e.g. 1: wanting to survey the eating habits of high school students across the country
						rather than doing every high school across the country, they survey all students from a few high schools(cluster)
					e.g. 2: A city government wants to assess the effectiveness of its public transportation system by surveying citizens
						cluster population by neighborhood and survey entire neighborhoods 

```ad-summary
title: Compare Stratified vs Cluster Sample
- Step 1 - Both putting people into groups
	- Stratified wants homogenous groups vs Cluster doesn't care
- Step 2 - Stratified selects individuals vs Cluster selects groups 
	- Stratified makes sure to pick individuals from every group
	- Cluster will select some groups and then ignore the rest 

 ##### Key Differences

- **Stratified Sampling**: The population is divided into subgroups (strata) that are meaningfully distinct, and a random sample is taken from **each stratum**.
- **Cluster Sampling**: The population is divided into clusters (which are typically geographically or naturally grouped), and **entire clusters** are randomly selected for the sample.

```

#### Non-Random Sampling (also called Non-Probability Sampling)![[Pasted image 20240906090346.png]]

- <mark style="background: #ABF7F7A6;">Convenience Sample</mark>
		- individuals nearby or in close proximity 
		- e.g. coffee shopping doing market research asking people who walk into the store/walk nearby
		- drawbacks: 
			- limited sample size
	- <mark style="background: #ABF7F7A6;">Purposive Sample</mark> (Expertise Sample)
		- e.g. looking for student experience on accessibility services on campus 
			- instead of asking random students, we ask students who have special needs 
		- BAD e.g. housing prices in Canada
			- looking at only BC and Ontario housing since they're the biggest markets
			- bad because housing prices will differ in Canada 
			- expertise but not a good use of it 
	- <mark style="background: #ABF7F7A6;">Snowball Sample</mark>
		- e.g. asking people for course opinions 
			- we ask past students if they found a course to be good or bad 
			- so we ask around for our friends and get more opinions by asking them for more of their friends to ask for opinions
		- a growing size of sample 
	- <mark style="background: #ABF7F7A6;"> Quota Sample </mark>
		- e.g. coffee shop owner wanting survey results of 15 people
			- employee asks first 15 people of the day
				- but these people are only in the morning
				- this misses out on people from the afternoon or evening
				- missing out on a large chunk of people and isn't a great representation 
		- limited effort/time; just wanting to hit quota 

```ad-summary
title: Probability vs Non-Probability Sampling
- Probability is ideal but can't always happen due to restrictions like time or money
- In short, **random sampling** is objective and unbiased, while **non-random sampling** is subjective and may introduce bias.

```

### Errors in Sampling
- <mark style="background: #ABF7F7A6;">Sampling Errors</mark> - These errors occur due to the fact that your sample is (almost always) smaller than the population
	- a biased representation of the population 
	- a sample is not fully representative of a population 
	- as long as you're doing sampling, this error will occur
- <mark style="background: #ABF7F7A6;">Non-Sampling Errors</mark> - These errors occur due to the human mistakes or machine malfunction by chance 
	- introduces mistakes or bias due humans error 

```ad-note
title: How do we reduce these errors?
Sampling Errors:
- making our sample size larger will give us a more complete representation
- similar proportions to the population

Non-Sampling Errors:
- hire people with necessary knowledge/provide training
- purchase good quality software/hardware 

```

#### Another way to classify the errors in sampling
- <mark style="background: #ABF7F7A6;">Errors of Inclusion</mark> - Some individuals are in the sampling frame but are not part of the population
	- these individuals may be included in the sample
	- this action of including them is a mistake
- <mark style="background: #ABF7F7A6;">Errors of Exclusion</mark> - Some individuals are not in the sampling frame but are part of the population
	- these individuals will never be included in the sample
	- this action of excluding them is a mistake 

```ad-example
title: Inclusion and Exclusion Error Example

Describe how errors of inclusion and errors of exclusion would occur in the following example: 

*"An online psychology therapy company plans to conduct a study about their users. The users include both the registered (paid) users and trial (free) users. The company is only able to reach the registered users to collect information. Suppose the target population for this study includes all the **individuals that used the company’s online platform over the past month**. The sampling frame is the list of 500 users registered with the company."*

Errors:
- Population 
	- people inside the population but outside the sample frame
		- e.g. active free trial uses (error of exclusion)
- Sampling Frame
	- people inside the sample frame but outside the population
		- e.g. inactive registered users (error of inclusion)
```

### Types of random variables

#### Categorical (also called qualitative) data
- the value of X is a label or word
- e.g. let X = the day of the week of a persons birth
- ![[Pasted image 20240911161206.png]]
	- in this case, the population is all births in the USA vs year 2020
	- a good way to present categorical data is with a bar plot (as shown above)
	- note that if X is a categorical variable, there is no such thing as the average value of X
		- however, we can determine the modal value of X
			- which in this case is Tuesday

##### Nominal variable (also called quantitative)
-  e.g. days in a week, colors, fruits, etc. 
- Nominal variables represent categories that do not have a specific order or rank
- Nominal data is about **classification**. You can only count how many observations fall into each category.

##### Ordinal variable
- e.g. google ratings, customer experience survey (e.g. rate from strongly disagree to strongly agree)
- Ordinal data shows **order**, but the difference between the ranks can’t be measured quantitatively.

#### Numerical data
- the value of X is a number
- e.g. let X = the age of a Canadian in 2010

##### Interval variable
- e.g. temperature
	- Temperature (Celsius or Fahrenheit): 20°C, 30°C (the difference between temperatures is meaningful, but 0°C doesn’t mean "no temperature").
- Interval data allows you to measure **differences**, but there’s no absolute zero.

##### Ratio variable
- e.g. mass, volume
	- if we say 0 kg, then that means there is no mass
- we have an absolute 0 
- Ratio data allows you to make comparisons using ratios (e.g., a person who weighs 100 kg weighs **twice** as much as someone who weighs 50 kg).

### Random variables and distributions
-  in stats, we are always interested in some variable X whose value depends on which individual we select from the larger population
	- such a variable X is called a **random variable**
- but being random doesn't mean that X is "totally random"
	- there is usually an underlying pattern to X
	- we can this the <mark style="background: #ABF7F7A6;">distribution</mark> of X 

```ad-example
title: Distribution Example
![[Pasted image 20240911162718.png]]

```

```ad-example
title: Distribution Example 2
![[Pasted image 20240911162750.png]]

```
