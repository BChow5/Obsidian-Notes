### Why probability?
- when we collect sample data and use it to calculate a statistic, the result depends on which random sample we happen to select 
	- the sample mean Xbar depends on which random sample was selected
- to make inferences about a population parameter, we need probability theory to answer questions like below
	- "Given a value for the population mean mew, is the observed mean xbar likely or unlikely?"

##### Example
Suppose you flip a coin 10 times, and get 4 heads and 6 tails

what is the sample proportion of heads, p hat? what does this suggest about the population proportion of heads, p?
- sample proportion = 4 head / 10 flip = 0.4
- you can't make a proper claim for the population proportion with just this info
- can't make a claim with statistical significance in this example
- suspect: by chance. still a fair coin

### Probability Theory Concepts
- <mark style="background: #ABF7F7A6;">random experiment</mark>
	- an experiment is called random if we can neither predict nor control which of the many possible outcomes of the experiment will occur
- <mark style="background: #ABF7F7A6;">outcome</mark>
	- one specific result for the random experiment
- <mark style="background: #ABF7F7A6;">sample space</mark>
	- the set of all possible outcomes for the random experiment
- <mark style="background: #ABF7F7A6;">event</mark>
	- a subset of the sample space (one or more outcomes for the random experiment)
	- an event, A, occurs if the outcome of the experiment is in A
- <mark style="background: #ABF7F7A6;">probability </mark>
	- associated with each event, A, is a probability P(A), which represents the chance that event A will occur 

##### Examples
2. Random experiment: flip a single coin

- sample space S = {heads or tails}
	- 2 outcomes

3. Random experiment: Roll a single die

- sample space S = {1,2,3,4,5,6}
- event of even roll = {2,4,6}

1. ![[Pasted image 20240927100829.png]]
	- Event A = roll of a sum of 7 ={(6,1),(5,2)...}

1. ![[Pasted image 20240927101029.png]]
	- Event B = select either a diamond card or a face card = all the diamond cards and all the face cards 
- the OR in stats means its inclusive. so either this, or that, or both.

### Two approaches to probability: classical vs relative frequency

#### Classical approach
![[Pasted image 20240927101754.png]]
P(X = 7) = 6/36 = 1/6 = 0.16667

#### Relative Frequency Approach
![[Pasted image 20240927101858.png]]
- If we were to increase the number  
of trials, then the simulated probabilities would gradually get closer to the theoretical values.  
This fact is called the <mark style="background: #ABF7F7A6;">Law of Large Numbers</mark> 

### Counting
#### Fundamental Counting Rule
![[Pasted image 20240927105257.png]]

#### Permutations
![[Pasted image 20240927105422.png]]
- for when order of the item matters and without replacement
#### Combinations
![[Pasted image 20240927105508.png]]
- for when order of the item doesn't matter and without replacement
- the denominator is for removing overcounting 
	- this is because we don't care about the order 

#### Summary 
![[Pasted image 20241002204523.png]]