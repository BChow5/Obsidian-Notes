### Complementary Rule
![[Pasted image 20241004083618.png]]
- classical approach of probability: P(A) = # number of outcomes A \ total # of outcomes = |A|/|S|
- Abar is the complement of A. the space outside of A but inside S 
- |A| + |Abar| = |S|
	- |A| = outcome of A
	- |Abar| = outcome of Abar
- |A|/|S| + |Abar|/|S| = 1
	- we divide by S to get the probabilities 
	- P(A) + P(Abar) = 1
		- 1 is the total event or the total sample space 
- P(A) = 1 - P(Abar)
	- you can use the compliment to find the probability of A and vice verse
	- this is the complementary rule 

### "at least one" problems 
- for at least one problems, it's easier to find Abar (opposite of the desired outcome) and then use that to calculate A (desirfed outcome) using the complimentary rule
![[Pasted image 20241004084244.png]]
- A = any birthday match
- Abar = everyone has distinct birthday

![[Pasted image 20241004085320.png]]
- P(any birthday match)
- 1 - P(Abar: all diff bdays)
	- 1 - # of all having diff bday / # of outcomes
	- 1 -  365 x 364 x... 359(7th person)/365^7
- order matters for this one and without replacement 
- 1 - 365P7 / 365^7
	-  = 0.056
- this is with the classical approach

![[Pasted image 20241004092846.png]]
- number of outcomes of drawing 5 cards = 52C5
- number of outcomes of drawing no aces in 5 cards = 48C5
- then we want to calculate the probability of drawing no aces (Abar) = 48C5/52C5
- probability of pulling at least 1 ace = 1 - probability of Abar

### Addition Rule
![[Pasted image 20241004101207.png]]
- union (U symbol) is "or"
	- "or" is the inclusive or
	- A or B or both 
- ![[Pasted image 20241004101504.png]]
- anything in E or L is the union
- |E U L|/|S| = |E|/|S| + |L|/|S| - |E n L|/|S|
	- |E n L| is the overlap of L and E. we remove it 
- **P(E U L) = P(E) + P(L) - P(E n L)**
	- = 1/2 + 1/2 - 2/6 = 2/3
$$ P(E\cup L) = P(E) + P(L) - P(E\cap L) $$
![[Pasted image 20241004102105.png]]
union (U symbol) is "or"
	- "or" is the inclusive or
	- A or B or both 
#### Mutually Exclusive Events
- if there is nothing in the intersection. the 2 events cannot happen at the same time 
- we can toss awaythe last part of the addition rule if they are mutually exclsuive
	- since there is no overlap we need to account for
- ![[Pasted image 20241004102136.png]]

![[Pasted image 20241004102232.png]]
![[Pasted image 20241004102311.png]]
- Definition 1
	- no overlapping
- Definition 2
	- |A n B| = 0
	- P(A n B) = 0
	- **P( A u B) = P(A) + P(B)**
		- this is the special addition rule for mutually exclusive events 
- examples
	- pass or fail the course 

### Multiplication Rule
![[Pasted image 20241004103153.png]]


![[Pasted image 20241004103206.png]]
- P(L1 n L2) = 3/27 x 2/26 = 0.00855
- **P(L1 n L2) = (L1) x P(L2 | L1)**
	- this means that L2 is dependent on the outcome of L1 
	- the | in this case represents a condition 
		- "given", "assuming"
	- given that we already have a left handed person, how likely are we to get a second left handed person 
	- this is saying that L1 is mandatory. it must happen 

![[Pasted image 20241004103755.png]]
- general version of the multiplication rule 
	- **P( A n B) = P(A) x P( B | A)**
- special version 
	- **P( A n B) = P(A) x P(B)
		- this is only for independent events
		- like when B does not depend on A 
		- e.g. like passing a class A and eating noodles B
- conditional probability 
	- **P(B|A) = P(A n B)/ P(A)**
	- |A n B|/|S| \ |A|/|S|
	- ![[Pasted image 20241004105913.png]]
	- how likely is B occurring in the overlap of A occurring 

![[Pasted image 20241004104406.png]]
- rainy (R) is 5% and sunny (Rbar) is 95%
- on rainy day - camping (C) 20% and not camping C(Cbar) 80%
- on sunny day - camping (C) 90% and not camping C(Cbar) 10%
- ![[Pasted image 20241004104744.png]]
- Tree diagram 
	- needs labels (either words or notation)
	- needs probability 
- **P(Rbar n C) 
	- = P(Rbar) x P(C | Rbar)**
		- the "n" = and
		- P(Rbar) = probability it isn't raining
		- P(C | Rbar) = probability of camping given it isn't raining
		- 95% x 90% = 0.865
- **P(R n C) 
	- = P(R) x P(C | R)**
		- P(R n C) = 5% x 20% = 0.01
- **P(C)**
	- this means we need to combine the possibility of both going camping in the rain, and when sunny 
	- these 2 events are mutually exclusive assuming it can't be rainy and sunny in the same day
	- we use a special version of the **addition rule**
	-  P(Rbar n C) + P(R n C) = 0.895
		- mutually exclusive
- **P(R | C)**
	- P(R n C) / P(C)
	- among the possibilities of camping, what is the possibility of it being a rainy day 
	- 0.01 / 0.895 = 0.0112

### Independent Events
![[Pasted image 20241004110701.png]]
-  P(L1 n L2) = P(L1) x P(L2 | L1)
	- = 10% x 10% 
	- there is so many people here that having 1 left person in here doesn't make much of a difference 
- P (A n B) = P(A) x P(B)

![[Pasted image 20241004111009.png]]
- definition 2
	- P(B) = P (B | A)
	- this is because B does not care about A
	- or you could say P(A n B) = P(A) x P(B)
- examples
	- eating noodles and passing an exam

![[Pasted image 20241004111903.png]]
- if mutually exclusive, they are dependent
- if 2 event

![[Pasted image 20241004111956.png]]
- ![[Pasted image 20241004112116.png]]
- P (F n I) =? P(F) x P(I)
	- P(F n I) = 4/17
	- P(F) = 7/17
	- P(I) = 10/17
	- 4/17 =? 7/17 x 10/17
		- they are not the same 
		- these 2 events are dependent 
- P( F n I) =? 0
	- 4/17 = 0
	- so these 2 are not mutually exclusive 