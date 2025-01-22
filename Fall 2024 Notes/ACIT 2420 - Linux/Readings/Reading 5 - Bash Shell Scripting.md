### Core Bash Script Ingredients 
- best practice: make sure your scripts always include the following
	- <mark style="background: #ABF7F7A6;">shebang</mark>: the indicator of the shell used to run the script code on the first line of the script
		- **#!/bin/bash** or **#!/usr/bin/env bash** to identify Bash as the script interpreter 
	- comment to explain what a script is doing 
	- white lines to increase readability
	- different blocks of code to easily distinguish between parts of the script 

#### About Script Names
- script names are arbitrary 
- extensions are not required but may be convenient for scripts users from non-linux operating systems 
	- use `.sh` extension for windows users to make it easy to recognize the script as a shell script 
	- `.csh` for c shell

#### About Script Location
- because of Linux $PATH restrictions, scripts cannot be executed from the current directory 
- consider using `~/bin` to store scripts for personal use
- consider using `/usr/local/bin` for scripts that should be available for all users 
	- you need sudo privileges to put it there 

#### Scripting file
- download git 
	- `sudo apt install git`
- start your script with the shebang
	- e.g. `!/bin/bash`
- comment for every code block where you switch functionality 
- last command in the script will always write the exit code 
	- e.g. `exit 0`
	- the exit code tells the script if it ran successfully 
- can run a shell script in two ways
	- put `bash` in front of the script file name in the terminal
	- `chmod` can change the permission mode
		- we add execute permission to it

### Running the Scripts
- to run a script as a separate program, you need to set the execute permission
	- `chmod +x myscript`
- if the directory containing the script is not in the $PATH, running it using `./myscript`
- scripts can also be started as an argument to the shell
	- in which case no execute permission and $PATH is needed: `bash myscript`
		- bash is the executable and the script is the argument to bash
- why wouldn't you just always use bash to run the script?
	- because of the shebang
	- maybe the script is written for another shell environment
	- using bash to run the script will then mess it up because of the shebang. it will ignore it

### Defining and Using Variables

#### Understanding Variables
- a local variable works in the current shell only
- an environment variable is an operating system setting that is set while booting
- arrays are special multi-valued variables 

#### Bash Variable Data Types
- bash variables don't use data types
- variables can contain a number, character or a string of characters
- in general, bash does not change the behaviour according to the type of data in a variable
- `declare` can be used to set specific variable attributes
	- instead of setting data types, we set attributes in bash
	- e.g. `declare -r ANSWER = yes` sets $ANSWER as a read only variable 
		- so it can't be changed
	- e.g. `declare [-a | -A] MYARRAY` is used to define an indexed or associative array
- using `declare` is NOT required to set a variable, but may be needed to set an array
- use `declare -p` to find out which type of variable something is
	- compare `declare -p GROUPS` with `declare -p PATH`

#### Defining Variables
- defining a variable is `key=value`
- variables are not case sensitive
	- environment variables, by default, are written in upper case
	- local variables can be written in any case 
- after defining, a variable is available in the current shell only
- to make variables available in subshell also, use `export key=value`
	- export is for subshells only
	- if you wanted a variable to be available in all shells, you should put it in a start file like `.bashrc`
- to clear variable contents, use `key=`
	- wipe the value of the variable without deleting the variable
	- you can use `unset` if you want to delete the variable also
- use `env` to get access to environment variables 

#### Using Variables
- to use the current value assigned to a variable put a `$` in front of the variable name
	- e.g. `echo $color`
- recommended (but not mandatory) to put the variable name between { }
	- e.g. `echo ${color}`
	- if you want to do operations on the value of a variable while working on it, then curly brackets are mandatory
- to avoid confusion on specific types of variables, it is recommended (not mandatory) to put variable names between double quotes
	- e.g. `echo "${color}"`
- if you have special characters like spaces then you should use the curly brackets

#### Special Variables
- bash works with some special variables, that have an automatically assigned value
	- `$RANDOM`: a random number
	- `$SECONDS`: the number of seconds this shell has been running
	- `$LINEO`: the line in the current script
	- `$HISTCMD`: the number of this command in history
	- `$GROUPS`: an array that holds the name of groups that the current user is a member of
	- `$DIRSTACK` list of recently visited directories, also use dirs to display

![[Pasted image 20241007191613.png]]
- best to put your variables at the top of the script

### Using Simple if Statements
- `if` is used to verify that a condition is true
```bash
if true
then
	echo command executed sucessfully
fi
```
- you can gave as much as you want in the if statement
- the if statement must be closed by an `fi`
- you can do nesting in if statements
	- like with while statements, etc.
- `then` introduces the commands we're going to run
- in a one liner, `then` can be on the same line
- the condition is a command that returned an exit code 0, or a test that completed successfully:
	- `if[-f/etc/host];then echo file exists;fi`
![[Pasted image 20241007192208.png]]![[Pasted image 20241007192257.png]]
- this hides the output of the grep since we don't care about that in this case
- `grep -s` would also do the same thing

### Testing with [[]]
#### Understanding `[[condition]]`
- a regular test is written as `[condition]`
- the enhances version is written as `[[condition]]`
- `[[condition]]` is a bash internal and offers features not offered by test
	- may not find it in other shells 
- many scripts prefer to use `test` instead due to lack of compatibility

#### `[[conditon]]` Examples
- `[[$VAR1 = yes && $VAR2 = red]]` is using a conditional statement within a test 
- `[[1<2]]` test if 1 is smaller than 2
- `[[-s $b]]` will test is $b exists
	- if $b is a file that contains spaces, `[[]]` don't require you to use quotes
- `[[$var=img* && ($var = *.png || $var = *,jpg)]] && echo $var starts with img and ends with .jpg or .png`
	- it's a double test that checking to find out if the file name starts with IMG and end with PNG or JPG. and if that is the case, it prints a message of $var starts with img and ends with png or jpg

### Using `if`...`then`...`else` with `elif`

#### Understanding elif
- elif can be added to the if...then...else statement to add a second condition
- ![[Pasted image 20241007201720.png]]
- ![[Pasted image 20241007201826.png]]
- many scripts try to avoid elif because it can get nested too much and be confusing 

### Using for
- for is used to iterate over a range of items
- this is useful for handling a range of arguments or a series of files
- ![[Pasted image 20241007202726.png]]
- ![[Pasted image 20241007202746.png]]
![[Pasted image 20241007203326.png]]