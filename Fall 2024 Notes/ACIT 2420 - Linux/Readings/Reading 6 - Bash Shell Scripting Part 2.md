### Using `shift`
- `shift` is used to shift the positional parameters to the left
- shift can take a number as its argument to shift the positional parameters to the left by that number
- using shift can be useful to remove arguments after processing them
- as an alternative, consider looping all arguments using while
- e.g. `shift 3` will move the parameter 3 to the left
- ![[Pasted image 20241014152000.png]]

### Using Here Documents
- a **here document** is used as Input/Output redirection to feed a command list to an interactive program or command 
- a here document is used as a scripted alternative that can be provided by input redirection 
- here documents are useful, as all the code that needs to be process is a part of the script, and there is no need to process any external commands
- The heredoc makes it easy to handle long strings of text in shell scripts or command-line input

#### example
![[Pasted image 20241014152218.png]]
- ENDSESSION is a keyword that marks the identifier for the here document 
- << is input redirection 
- you should read this as, event every single line of input until the keyword is found

![[Pasted image 20241014152821.png]]


### Using Functions
- function is a small block of reusable code that can be called from the script by referring to its name
- using functions is convenient when blocks of code are needed repeatedly
- functions can be defined in 2 ways:
```bash
function_name () {
	commands
}
```

```bash
function function_name {
	commands
}
```

#### Managing Functions
- functions are local to the script where they are executed
- bash-wide functions are initialized from the bash startup scripts
- use `set` to show a list of all functions currently available
- remove a function by `unset`

#### Using Function Arguments
- functions can work with arguments, which have a local scope within the function
- ![[Pasted image 20241014154321.png]]
	- here the function refers to bob as $1 so it will substitute it into the command in your function hello
	- could also be done with command line arguments rather then defining it in the script
- function arguments are not affected by passing positional parameters to a script while executing it 
![[Pasted image 20241014154650.png]]

### Using Case
- case is a more efficient way of doing if and else statements
- `case` is used to check a specific number of cases
- useful to provide scripts that work with certain specific arguments
- has been used in a lot of legacy Linux init scripts
- case is case sensitive
	- consider using tr to convert all to a specific case

![[Pasted image 20241014155545.png]]
	- note that this script has some errors in it
	- (yes | oui) is taking care of 2 cases in 1 line\
		- you use pipes as a separator for this. not commas 
	- you define the cases to be checked on one line
		- with a closing curly brace
		- there is no opening one
		- after the case, then it does the command
	- the wild card case checks for everything else
	- case `<condition>` in 
		- then you define all your other cases
		- good to include the wildcard case
		- `esac` to close the case
- you can nest if and other cases in case

### Working with Options
- an option is an argument that changes script behavior
	- e.g. -l
- use **while getopts "ab:" opts** to evaluate options a and b, and while evaluating them, put them in a temporary variable opts 
	- getopts is evaluating the string and its doing a while iteration over it
		- so it will consider them one by one and put them in the temporary variable opts 
	- now in this example, "ab:" which means that A is a straight options, which means you use it as -a. b is an option that can work with an option argument 
	- an <mark style="background: #ABF7F7A6;">option argument</mark> is referred as a specified of the option 
- if you want to handle options, then you are going to work with while
- you are going to use the built-in getopts
- next use `case $opts` to define what should happen if a specific option was encountered 

#### Examples
![[Pasted image 20241014161619.png]]

### Using Variables in Functions
- no matter where they are defined, variables are always global scoped
	- even when defined in a function
- use the `local` keyword to define variables with a local scope inside of a function

#### Example
![[Pasted image 20241014162015.png]]
- var2 will print B when inside the function
- var2 prints nothing when called outside the function 