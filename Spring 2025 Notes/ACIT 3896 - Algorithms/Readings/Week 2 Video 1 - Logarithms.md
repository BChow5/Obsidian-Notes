- logs are the opposite of exponents

e.g. 1 
- e.g. 3<sup>4 </sup>= 81
- log<sub>3</sub>(81) = 4
	- 3 is the base

e.g. 2
- 4<sup>5</sup> = 1x 4 x 4 x 4 x 4 x 4
- log<sub>4</sub>1024 = 5
	- this is saying, "if i start at 1 and want to get to 1024... how many times should we multiply by 4?"
	- log is getting from 1 to 1024, going by 4 (the base). how many does it take?
	- if you want to go from 1 to X, by multiplying by B, how many multiplications does it take?
- also means that 1024 /4 /4 /4 /4 /4 = 1 

e.g. 3
- popular bases: e, 10, 2
	- in this class base 2 is our favourite 
- when you change base, you just need to multiply it by some number
	- the ratio is the same for all of them 
- on a calculator, ln is log natural 
	- this gives yo u log base e
- log<sub>b</sub>x = log<sub>q</sub>x
	- we need the scale factor to convert b to q 
	- we can get it with just **log<sub>q</sub>x/log<sub>q</sub>b to get the scale factor**
		- b is the base we wanted
		- q could be 10 or e
	- so in a calculator it would be "log X / log B"

#### Review
- logarithms are reverse exponentiation (but they're not roots)
	- and they're the important reverse exponentiation for algorithms
- (the other reverse is root but we don't really care about that)
- logarithm asks "if you want to go from 1 to X, by multiplying by B, how many multiplications does it take?"
- all of the math works our fine with fractions/floats 
- the "base" is the subscript in log
- result goes like a functional argument 
- getting up to X from 1 by multiplying by 2 is the same as getting to 1 from X by dividing by 2
- change base (for calculators) is figuring out the scale factor 
- for comp sci, base is usually 2  