### The Linux Processes

#### The Linux boot process
- UEFI/BIOS
	- power on self test (POST)
		- checking your hardware before startup
- Boot Loader
	- GRUB2, systemd-boot
		- systemd is lighter and faster
		- GRUB@ has more options but slower
- kernel 
- init
	- systemd, runit, r6, openRC
	- started by kernel

#### What is a process?
- a running instance of a program (executable file)

#### the `/proc` directory
-  this directory contains information on running processes on our machine

#### exit codes or exit status
-  when a process termiantes for any reason, it returns an exit code
- you can view the exit code og the last command with `echo $?`
- 0 indicates success. the command did what it was told to do (this isn't the same thing as doing what you want it to do)
	- then it exited without error
- none 0 indicates an error
- there is no standardization for exit codes other than 0 means no error
Let's try this out.

- run any command, like `ls`
- after run the command `echo $?`
    - You should see `0`
- Next run the command `sweatyklingons`
- Then one more time `echo $?`
    - You should see `127`

We will revisit this when we start writing shell scripts.

> [!note] Reminder You can run previous commands in your shells history by pressing the up arrow key and down arrow keys to move backwards and forwards one command at a time. Or use `ctrl+r` to search through your history and run previous commands.

#### Signals 
- signals were part of the original Unix OS and now they are included in all of the unix-like OSs (Linux, BSD, MacOS)
- signals provide a way for a user, or another process, to communicate with a running process
- there are a lot of signals but we will look at these 3
	- SIGNIT 2
	- SIGKILL 9
	- SIGTERM 15
- **SIGINT** is the signal sent when you press `ctrl+c`. This attempts to interrupt a foreground process. Like when you have a python app running.
- **SIGTERM** attempts to terminate a process. This is like a graceful shutdown. It will allow a process to finish what it is doing, stop child processes of this process... This is the preferred way to stop a process.
- **SIGKILL** forces a process to quit immediately.

#### discovering processes with the `ps` utility 
- the `ps` utility allows you to take a snapshot of running processes. when you run ti without arguements, it will deiplay something like this:
```bash
   PID TTY          TIME CMD  
 97350 pts/0    00:00:00 bash  
109522 pts/0    00:00:00 ps
```
- PID - the process ID (every process is assigned a number as an ID)
- TTY - which TTY (terminal screen) the process is tied to, or `?` for none
- TIME - the accumulated CPU time the process has used
- CMD - the name of the command 
-  this isn't all that useful as we just see the ps command itself and the shell it was launched from
- to see every process, we can use the -e option `ps -e`
	- with the `-e` option, the `ps` utility shows us too much info
- we can combine `ps` with other utilities to help us better find the process info that you are looking for with a pipe
- pipe the output of one command into another command. 
	- Pipes are written using the the "pipe" character "|".
- The command below pipes the output of `ps` into `less`. `less` is the pager that we have been using with the man pages. A pager just shows information a screen at a time, or a page at time.

```bash
`ps -e | less`
```

- The advantage of this is that I can then search for specific information using "/". Try typing `/bash` and press "Enter". This will take you to the first instance of a matching search and highlight it.
- I can also move around in the document using vim motions (j and k, gg and G...).
- To close `less` press `q`.

A more common way to find the results you are looking for is with `grep`.
```bash 
`ps -e | grep -i bash`
```

We can also specify the data that we want with the -o option, this allows the user to format the output. You provide the -o option with a comma separated list of arguments for the output that you want displayed.

```bash 
`ps -eo comm,pid,%cpu | grep -i systemd`
```

To see a complete list of possible arguments look at the "STANDARD FORMAT SPECIFIERS" section in the ps man page.

Another useful argument is the -H or --forest options. These can both be used to display a hierarchy of processes. You will be able to see which processes were started by another processes. These will show slightly different output:

- -H uses indentation
- --forest indentation and the characters. `\_`.

```bash 
`ps -e --forest | grep -i ssh`
```

#### Terminate a process with `kill` and `pkill`
-  The `kill` command is actually a shell builtin in several Linux shells including bash and zsh
- `pkill` is a little easier to use, because you use a pattern that matches the process name instead of PID
- e.g. if I want to kill Chrome, I could use the command `pkill Chrome`. If however, I have two or more processes _Chrome_ and _Chrome-browser_ for example, that match a pattern and I only want to kill one of them I need to be careful.
- You could use the regular expression `pkill '^pattern$'`
	- regular expressions get used to finding matching for the pattern
	- like it matches with just the word pattern 
- Or in some case you can specify by user `pkill -u user-name pattern`
- Finally if I want KILL a process, instead of TERMINATING a process I could use  `pkill -9 pattern`

Example of kill command
```bash
kill [OPTIONS] [PID]...
kill -9 $(pidof firefox) # this is an example of command substitution
```

example of pkill command:
```bash
pkill [OPTIONS] <PATTERN>
pkill -15 Chromium
```

### Finding files with `find` and `grep`

#### Finding files with `find`
- the `find` utility can be used to find files based on file properties
	- the name of the file, the size. when it was last modified

```bash 
`find [options] [path...] [expression]`
```
##### -exec

- `-exec <command> {} \;` use this to execute a command on files found by find 
	- executes right away when it finds the files 
- `-exec <command> {} +` same as above, but collect files, then run command.
	- finds all the files first then it does the command  
		- This option will involve fewer invocations of the command.

##### [](#examples)examples

`find ~ -type f` find all of the regular files in your home directory

`find . -type f -name "*.txt"` find all files in the current directory, or sub directory that end in ".txt".

`find . -type f -mmin -10` find all of the files in the current directory that were modified in the last 10 minutes (less than 10 minutes ago)

`find . -type f -mmin +10` same as above, but will find files modified more than 10 minutes ago.

`find / -size +10M` find files larger than 10 Megabytes anywhere in the root directory or sub-directories.

##### -ctime vs -mtime
- -mtime is the file contents.
- -ctime is file metadata and contents, permission for example.

#### Find files with `grep`
- `grep` will find stuff based on what is inside the file 
	- the contents inside the file
- `g/re/p` (_**g**lobal / **r**egular **e**xpression search **/** and **p**rint_)
- The `grep` utility can be used to find patterns of text in a file(s).

```bash 
`grep [_OPTION_...] _PATTERNS_ [_FILE_...]`
```

This has a few uses, an obvious one is looking for a file based on the contents of that file.

Perhaps a less obvious use of grep is to _filter_ the output of another command. You can do this with the pipe character.

`ps -eo comm,rss | grep bash`

##### examples

Remember that most server versions of a Linux OS will have minimal features. One of the niceties that most work station Linux OSs have is colored output for grep. You can achieve this with `--color=always`. See the man page for grep for more.

`grep search-word file` basic grep command

`grep -iw search-word file` search only for the search word, not sub-strings, and ignore case

`grep -r search-word directory` search recursively for all instances in a file. -r and -R are slightly different, see the grep man page for more

`grep -C 2 "search words" file` return found instances of a search term with 2 lines above and below where the search pattern was found, for context.

`grep -c "set" ~/.bashrc` count the number of times "set" is found in our .bashrc file

`grep -rl "search pattern" dir` return a list of files that contain the search pattern

`fc-list | grep -i "nerd"` find all of the nerd fonts you have installed on your system uses a pipe to filter the output of the `fc-list` command.

#### regular expressions

You will probably learn more about regular expressions in one of your programming courses. Regular expressions can get gross. I recommend finding a good cheat sheet and add that to your notes.

`grep -P "\d{2}" file` this uses the -P option which tells grep to use a perl compatible regular expression. Not all versions of grep have this option.

`grep "[0-9]{2}" file` will do the same as above

`grep -P '\(\d{3}\) \d{3}-\d{4}|\d{3}-\d{3}-\d{4}' file` This will match phone numbers written as: (555) 555-5555 or 555-555-5555 in a regular expression '|' is or