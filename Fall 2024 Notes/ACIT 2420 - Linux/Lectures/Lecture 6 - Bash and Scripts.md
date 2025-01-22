### The Shebang
- first line of the shell script
- tells the shell which interpreter to use when the script is executed 
- two ways to write a shebang:
	- `#!/bin/bash`
		- absolute path to the interpreter 
	- `#!/usr/bin/env python`
		- using the env utility to find the interpreter
```bash
#!/bin/bash

#: Title       : hw
#: Date        : Oct 09 2023
#: Author      : Nathan McNinch
#: Version     : 1.0
#: Description : print Hello, World!
#: Options     : None
echo "Hello, World!"
```

### Single quotes vs double quotes
- double quotes expand
- single quotes are literal 
	- will literally do what is inside the single quotes

```bash
animal=cat
echo "$animal"
cat
#reads the value inside of animal by expanding it 


echol '$animal'
$animal #literally printing animal
```

### echo
- the built in `echo` writes arguments to standard out
- `echo` prints a `\n` a new line after the echo line is executed 
- echo can be used to print multiple lines and will preserve white space

### Variables in bash
- variables can be declared like this: `animal="wolf"`
- to use a variable in your code prepend a $
	- `echo "$animal"`
- when calling a variable it will look at these 3 places to find it
	- local shell variables
	- environment variables (PATH)
	- functions 

#### environment variables
- environment variables are accessible in sub shells
- `export variable=value` to make a environment variable
- using uppercase for environment variables is a convention like PATH and EDITOR

### the `declare` built-in
- you can use the `declare` built in to provide attributes to a variable
```bash
# declare a read only variable
declare -r CONFIG=file

# declare a variable as an integer
declare -i NUM
```

### Positional Parameters
- `$1` is a "positional parameter"
- these access arguments passed into the script when you run it 

```bash
#!/bin/bash
echo $0 $1 $2
```
- $0 is the path to the script
- $1 is the first argument
- $2 is the second argument 

#### Special Variables
![[Pasted image 20241008142142.png]]

### Conditionals
- The `test` builtin can be used to evaluate an expression.
```bash 
`test "var1" operator "var2"`
```

for example
```bash
`test 100 -ge 10 && echo "True" || echo "False"`
```

- these can also be done with if else statements
	- this is recommended for us to use 
```bash
if [[ test ]]; then
	consequent
fi
```

- bash also includes `elif` and `else`conditions
```bash
if [[ test ]]; then
	consequent
elif [[ second test ]]; then
	consequent
else
	consequent
fi
```

- for example, we can test if one file is older (based on last modified date) than another file
```bash
if [[ file-one -ot file-two ]]; then
  echo "file-one is older than file-two"
fi
```

#### `[[]]` vs `[]`
- `[[` has more features `[` is more portable. 
- Conditions written with a single square bracket will run in more shells. 
	- Since we are only going to be writing bash, just always use the double square brackets.
- pretty much always use them for the if statements 

### Loops
- bash has for, until, and while loops 

#### Indexed and associative arrays 
- bash includes indexed arrays and associative arrays
- bash arrays can NOT be more than one dimensional, so you can't nest an array inside an array

creating an **indexed array**:
```bash
declare -a arr=("1" "2" "3")
# or
arr=("1" "2" "3")
# Array items are separated by whitespace
```

accessing array values
```bash
#!/usr/bin/env bash
arr=("1" "2" "3")
# access by index
echo "${arr[0]}"
# print all of the array items
echo "${arr[@]}"
# print number of array items
echo "${#arr[@]}"
```

creating an **associative array**
```bash
declare -A person
person["name"]="Jan"
person["age"]="31"
```

accessing values and keys in an associative array
```bash
# print all values
echo "${ex_arr[@]}"
# print all keys
echo "${!ex_arr[@]}"
# print value by key
echo "${ex_arr["key1"]}"
```

### for loops
- basic syntax for a for loop
```bash
for item in [LIST]; do
  [COMMANDS]
done
```

- you can also write c style for loops
```bash
for ((i = 0 ; i <= 1000 ; i++)); do
  echo "Counter: $i"
done
```

what does this do?
```bash
#!/bin/bash
for i in {01..10..2}; do
  echo $i
done
```
- checks in a range from 1-10 by increments of 2
- prints i on every increment of 2

### break and continue
- bash includes `break` and `continue`
- these can be used to exit from a loop entirely or exit from the current loop iteration 
- **Break**
	- Exit from a `for`, `while`, `until`, or `select` loop.
- **Continue**
	- Resume the next iteration of an enclosing `for`, `while`, `until`, or `select` loop.

### command substitution
- in the shell, you can store the output of another command in a variable using command substitution 
```bash 
`$(command)`
```

for exmaple, we can store the output of a date command in a variable like this:
```bash
`now=$(date)`
```

### File extentions
- generally shell scripts have no file extension
### Additional resources
Google Bash Style Guide
https://google.github.io/styleguide/shellguide.html

bash scripting cheat sheet
https://devhints.io/bash
#midterm 
