### Shell Expansions
- Shell expansions allow the shell (like Bash) to transform text, substitute variables, and interpret commands
- expansions are performed on the command like after a command has been split into **token(s)**
- there are seven kind of expansions performed
	- brace expansion
	- tilde expansion
	- parameter and variable expansion
	- command substitution
	- arithmetic expansion
	- word splitting
	- filename expansion

#### Brace Expansion
- brace expansions allows you to generate a sequence of string by expanding a pattern within curly braces
- useful for creating sequences or generating multiple variations of a string
```bash
echo {1..5..2} # returns 1 3 5
echo {a..f..2} # returns a c e
echo file{1..3} # returns file1 file2 file3
echo file{1,2,3}.txt # returns file1.txt file2.txt file3.txt
```

#### Tilde Expansion
-  `~` is the tilde expansion
	- to represent the home directory of the current user or other users 
- `~` -> `/home/user-name`

#### Parameter and Variable Expansion
```bash
first="Nathan"
last="McNinch"
echo "${first}_${last}" # returns Nathan_McNinch
```
- Parameter expansion allows you to reference the values of variables
- using curly braces around the variable name can help prevent ambiguity and clarify where the variable name ends

We can also use it to replace characters
```bash
letters="abc"
echo ${letters/abc/123} # returns 123
```

Perform strings slicing
```bash
name="Nathan"
echo "${name:0:2}" # returns Na

echo "${name:2:2}" # returns th
```

#### Command Substitution
- command substitution allows the output of a command to replace the command itself
```bash
now=$(date)
dir=$(realpath "$1") # use realpath to convert a relative path to an absolute path
```
- Command substitution allows you to run a command and use its output as part of another command
```bash
echo "Current directory is $(pwd)" # prints Current directory is /home/user
```

#### Arithmetic Expansion
- allows the evaluation of an arithmetic expression and the substitution of the result
- the format for arithmetic expansion is: 
```bash
$(( expression ))
```

```bash
echo "$(( (3+4) * 5 ))" # returns 35
```

#### Filename Expansion 
- bash scans each word for the characters "`*`", "`?`", "`[`". if one of these characters appears and is not quoted, then the word is regarded as a pattern, and replaced with an alphabetically sorted list of filenames matching the pattern
- `*` matches any sequences of character
- `?` matches any single character
- `[]` matches any enclosed characters
```bash
ls d* # will display the directories starting with d

ls dir? # will display dir1 and dir2

ls *[1-2] # will display all of the directories that end with 1 or 2
```

### Functions in Bash
- a shell function is a compound command that has been given a name
	- it stores a series of commands for later execution
	- the name becomes a command in its own right and can be used in the same way as any other command
	- its arguments are available in the positional parameters, just as any other script
- while bash does have a function keyword, it is rarely used

you could write functions like this:
```bash
function func_name() {
  function body
}
```

**but most of the time you will see this format to write a function:**

```bash
func_name() {
  function body
}
```
- functions in bash must be declarred before the are called
- arguments are passed into functions using positional parameters 

```bash
# declare a function
say_hi() {
  echo "hi $1"
}
# call a function
say_hi "Bob" # returns hi Bob
```

#### Local Variables
- by default, all variables in bash are global variables
	- even variables in functions
- variables can be defined as local to function scope using the `local` keyword

```bash
#!/usr/bin/env bash
localretrn () {
  local func_result="some result"
  echo "$func_result"
}

func_result="$(localretrn)"
echo $func_result
```

```bash
#!/usr/bin/env bash

# This will print arguments passed to the function
printall() {
  local names=$@
  for name in $names; do
    echo "arg to func: $name"
  done
  echo "Inside func: \$0 is still: $0"
}

# This passes all positional parameters to the printall function
printall $@
```

#### Returns a Value from a Bash Function
- bash does include a `return` keyword but it isn't used to return a value from a function
	- it is used to exit a function and return an exist status. a number from 0-255
- if you want to return a value from a function, you would do so like this:
```bash
function add {
    echo $(( $1 + $2 )) # print the value with echo
}

result=$(add 3 5) # capture the value printed in a new variable
echo "The result is: $result"
```

### `Case` statement in bash
- similar to if/then/else clauses 
- `case` comes with powerful pattern matching and is very useful in scripting 
- `case` statements are used for conditional branching based on pattern matching
- A `case` statement allows you to execute different blocks of code depending on the value of a variable or expression.

```bash
case EXPRESSION in

  PATTERN_1)
    STATEMENTS
    ;;
    
  PATTERN_2)
    STATEMENTS
    ;;
    
  *)
    STATEMENTS
    ;;
esac
```

the following example uses the `read` built in to `-p` prompt the user to enter a character and store the entered character in the character variable 

```bash
#!/usr/bin/env bash
# Use read with -p option to prompt the user to enter a character
# store the character in the "character" variable
read -p "Enter a character " character

# See if character matches one of the cases on the case statement
case "$character" in
  A)
    echo "You entered A"
    ;;
    
  B|b)
    echo "You entered B or b"
    ;;
    
  C)
    echo "You entered C"
    ;;
    
  [1-3])
    echo "You entered 1 2 or 3"
    ;;
    
  *)
    echo "You did not enter A, B, b, or C or 1, 2, or 3"
    ;;
esac
```
- `)` indicates the test. i.e. the pattern to try to match
- `;;` indicates the end of the condition
- `*` matches any string of characters. use for default case
- `?` matches any single character
- you can use a range `[a-z]` in a case statement
- `|` separates alternative choices that satisfy a particular branch of the case structure 

### Use `getopts` to Write a Script That Has Options
- `getopts` is a shell built in that can be used to aide you in your handling options in your shell scripts
- `getopts` is a function that takes 3 arguments
	- valid options that will be handled 
		- e.g. `"ab"` in the example below
	- a variable populated with the option arguments
		- e.g. `opt` in the example below
	- a list of options and arguments to be processed `$@`

```bash
#!/usr/bin/env bash
while getopts "ab" opt; do
  case "${opt}" in
    a)
      echo "you supplied option -a"
      ;;
    b)
      echo "you supplied option -b"
      ;;
    *)
      exit 1
      ;;
  esac
done

exit 0
```

#### OPTARG
- when a flag is set to expect an argument, the arguments for that flag are held in the OPTARG variable
	- e.g. if you run a script `my_script -n "ted"` the OPTARG for -n is the string "ted"
- `OPTARG` is a special variable used in conjunction with `getopts` to store the argument that follows an option when an option requires one.
- - The string `"abf:"` tells `getopts` that `-a` and `-b` are options with no arguments, but `-f` requires an argument (because of the colon `:` after `f`).
- If the user provides `-f` followed by a value, that value is stored in `OPTARG`.

```bash
#!/bin/bash
vara=""
varb=""
while getopts ":a:b:" opt; do
  case "${opt}" in
    a)
      vara=${OPTARG}
      ;;
    b)
      varb=${OPTARG}
      ;;
    :)
      echo "Error: -${OPTARG} requires an argument"
      exit 1
      ;;
    ?)
      exit 1
      ;;
  esac
done

if [[ -n $vara ]]; then
  echo "$vara"
fi
if [[ -n $varb ]]; then
  echo "$varb"
fi
```
- The first colon in the options string below tells getopts to run in silent error checking mode. Or let the script define errors.

### `shift` Positional Parameters
- `shift` positional parameters *n* characters to the left
	- the default is 1 character

```bash
#!/bin/bash
echo $1 $2 $3 # prints 1 2 3
shift
echo $1 $2 $3 # prints 2 3 4
shift 2
echo $1 $2 $3 # prints 4, it shifts 2 additional positions
```

### OPTIND
- when dealing with a mix of `getopts` and positional arguments, **the flags and flag options should always be provided before the positional arguments**
	- this is because we want to parse and handle all flags and flag arguments before we get to the positional arguments 
- `OPTIND` is a special variable used with the `getopts` function to keep track of the index of the next argument to be processed

The line below shifts the positional parameters to remove any options handled by `getopts`.
```bash
`shift $(($OPTIND -1))`
```

This also works if your options accept arguments.

```bash
#!/bin/bash
vara=""
varb=""
while getopts ":a:b:" opt; do
  case "${opt}" in
    a)
      vara=${OPTARG}
      ;;
    b)
      varb=${OPTARG}
      ;;
    :)
      echo "Error: -${OPTARG} requires an argument"
      exit 1
      ;;
    "?")
      exit 1
      ;;
  esac
done

shift $((OPTIND - 1))

# check if there are positional parameters
if [[ $# -lt 1 ]]; then
  echo "you need to provide an option -a and or -b with an argument"
  exit 1
fi

# print positional parameters
echo "$1"
if [[ $vara != "" ]]; then
  echo "$vara"
fi

if [[ $varb != "" ]]; then
  echo "$varb"
fi
```