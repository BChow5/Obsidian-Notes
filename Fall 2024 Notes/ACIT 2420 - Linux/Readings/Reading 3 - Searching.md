### File Globbing

#### Wildcards

| Wildcard | matches                 | example       | explanation                  |
| -------- | ----------------------- | ------------- | ---------------------------- |
| ?        | a single character      | ls b?b.txt    | matches bob.txt and bab.txt  |
| *        | zero or more characters | ls bo*.txt    | matches bob.txt and both.txt |
| [ ]      | letters inside brackets | ls [bs]ob.txt | matches bob.txt and sob.txt  |
- brackets can also contain ranges
	- e.g. 
		- [a-z] matches lower case alphanumeric letters 
		- [0-9] matches numbers 0 through 9
		- [0-9t] matches numbers 0 through 9 and the letter "t"

##### Examples

| `ls invoice-*-2023*.pdf` | list files starting with the word "invoice-", followed by any letters, followed by "2023" followed by any letters, ending with ".pdf" |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `nano *2023-0[3-4]*.csv` | edit files starting with any letters, followed by "2022-0" followed by 3 or 4, followed by any letters, ending with ".csv"            |
| `ls b??.txt`             | lists files starting with the letter "b" containing only 3 letters before ending with ".txt"                                          |
#### Find 
- find directory entries matching criteria
- can specify by name, date, size, type, and more
- ![[Pasted image 20240924232550.png]]
- ![[Pasted image 20240924232641.png]]

##### Criteria: -type
- f = file
- d = directory 
-  l = symbolic link
- e.g. find -type d 
	- finds all directories under your home directory

##### Criteria: -name
- -name {wildcard name specified}
- e.g. `find -name "*neon*"`
	- finds all filesystem items with the text "neon" anywhere in the name 

##### Criteria: -size
- -size {size specifier}
- e.g. find -size +10G
	- finds files 10gb or bigger

##### Criteria: -mtime
- -mtime {number of days}
- e.g. find -mtime -14
	- finds all filesystem items whose contents have been modified in the last 14 days 

#### Grep
- used to search for string by patterns called regular expressions
- ![[Pasted image 20241001215200.png]]


| option   | meaning                                                      |
| -------- | ------------------------------------------------------------ |
| -r       | recursive - search whole directory tree (don't follow links) |
| -R       | recursive - search whole directory tree (follow links)       |
| -i       | ignore case of text                                          |
| -n       | print line numbers                                           |
| -v       | invert match - print lines that do not match                 |
| and more |                                                              |
![[Pasted image 20241001215420.png]]
#### Find and grep together
![[Pasted image 20241001215537.png]]
