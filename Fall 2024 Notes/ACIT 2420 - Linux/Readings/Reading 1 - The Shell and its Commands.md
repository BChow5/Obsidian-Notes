### What is a Shell?
- <mark style="background: #ABF7F7A6;">Shell </mark>makes an operating systems system's services accessible to users or other programs
	- program that receives commands and sends them to the operating system for processing
	- user has the option of typing commands from the keyboard, or they can be written in a shell script that can be reused
- fundamental capability of shells is the ability to launch command-line programs that are already installed on the system
- Linux users can utilize a variety of shells 

#### Shell Built-in Commands
- When you type a command in the terminal, the shell (which is the program that runs commands) will first check if it's a command that is built into the shell itself. 
- If it’s not, the shell will then look through a list of folders (called the search path) to find the command you want to run. 
	- You can see which folders the shell searches by typing echo $PATH. The search path includes important system folders and sometimes your home folder, but not always the current folder you’re in.
	- You can also create your own programs and run them by just typing their names, as long as they’re saved in one of those folders. 
	- For example, if you save your program in the bin folder, the shell can always find and run it, no matter where you are. 

### Basic Shell Commands
#commands
- pwd - print working directory 
	- pwd command determines which directory you are in
- mkdir
	- mkdir command to make a new directory 
- rmdir
	- deletes a directory
	- can only be used to remove empty directories
	- To remove files and directories, use **rm -rf directoryname/** (where **–rf** will recursively remove all the files and directories from inside the directory).
- touch
	- used to make empty files
- ls
	- ls command to see all files and directories inside the directory you're in
	- ls -a command to see hidden files
- cp
	- copy files from the command line 
	- requires two arguments
		- first specifies the specific location of the file and second specifies where to copy
	- e.g. cp file1.txt testfolder/
- mv
	- mv command to move a file or directory from one location to another or even to rename a file 
	- e.g. - **mv file1.txt file2.txt**
- rm
	- remove files or directories 
		- use -r or -f to recursively remove a directory (-r) or force remove a file or directory (-f)
- locate
	- used to find the location of a file
	- -i argument helps to ignore case sensitivity 

### Intermediate Shell Commands
#commands 
- echo
	- allows you to display content that can be added to either a new or an existing file or to replace content
- cat
	- cat command is used to read the content of a file
	- you can use cat command and append the output of a new file using >>
- df
	- lets you see the overall disk size, amount of space used, and the amount of space used, the amount of space available, the utilization percentage, and the partition that the disk is mounted on
	- use -h parameter to make the data legible by humans
- du
	- shows the size of a specific directory or subdirectory
- uname
	- displays info regarding the operating system that your Linux is on
- chmod
	- system call and command used to odify the special mode flags and access permissions of file system objects 
	- can change permissions for read,write, and execute 
	- The symbolic permissions notation is used in this example. **u**, **g**, and **o** stand for _user_, _group_, and _other_, respectively. The letters **r**, **w**, and **x** stand for _read_, _write_, and _execute_,
	- **4** stands for read, Write has the prefix **2** ,**1**  denotes _execute_ ,**0** means _no authorization_ 
		- **7** is made up of the permissions **4**+**2**+**1** (read, write, and execute), **5** (read, no write, and execute), and **4** (read, no write, and no execute).
- chown
	- change owner of the system files and directories on Unix
- chgrp
	- a filesystem object's group can be changed to another one 