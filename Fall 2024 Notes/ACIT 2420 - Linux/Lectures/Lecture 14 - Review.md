### General
- stderr code = 2
- remove a package and its dependencies: sudo pacman -Rns package
	- `-R` Removes the specified package(s).
	- `-n`: Removes the package without saving its configuration files, ensuring a clean removal.
	- `-s`: Recursively removes all unneeded dependencies that were installed with the package, provided no other installed packages need them.
### week 02

- SSH keys
- Intro to digital ocean
- Intro to Vim

#### Things you should know:

- What are ssh keys, where are they used and why
- Where are ssh keys on your local machine, what about on the server
- General method of communication
	- what is happening when you make an ssh connection?
- ssh command
- ssh-keygen, creating ssh keys
	- how do we create keys?
	- where do keys go?
- Basic vim movments
    - up, left, right, down
    - bottom of a buffer, top of a buffer
    - start of a line end of line
- modes
- command
    - quit
    - save

### [](#week-03)week 03

- Linux file system
- cloud-init

#### Things you should know:

- frequently used directories and files
    - /etc
        - /etc/passwd
        - /etc/shadow
    - /usr
        - /usr/bin vs /bin
    - /bin
    - /home
        - know where the home directory is for a user vs root
- check `man hier`
	- general info about the hierarchy 
- file system hierarchy general info
- what is cloud-init, why is it useful
    - languages used for configuration
    - general, how does it work?

### [](#week-04)week 04

- processes
- ps
- find
- grep

#### Things you should know:

- What is a process?
- How are commands run in the shell
	- what is the process of running a command?
- ps
    - `-e`
    - `-o`
- general find and grep commands
    - How do you search for a file
    - How do you search for a pattern in a file

### [](#week-05)week 05

- users
- groups
- permissions
- Neovim config example

#### Things you should know:

- relationships between users groups and permissions
- commands for managing users groups and permissions that we have looked at in class
    - chmod
    - chown
    - useradd
    - usermod
    - userdel
    - groups
    - groupadd
- Where is your Neovim config

### [](#week-06)week 06

- intro to bash scripting
- variables
- conditionals
	- e.g. be able to check if a file or directory exists
- loops
	- e.g. loop through an array

#### Things you should know:

- write variables and environment variables
- write a conditional that will check if a file or directory exists
- write a for loop that will loop over an array
- what is a shebang? Why not just run scripts with `<shell> <script>`?
- When you run a command where does the shell search for that command, in what order?
- bash configuration files where and when

### [](#week-07)week 07

- bash expansions
- bash functions
- bash case statements
- handling options with getopts

#### Things you should know:

- What is an expansion (general)
- know why var=val works vs var = val
- ${var} vs $var
- function arguments and returns, general function syntax
- general case statement syntax
- what is getopts, general how does getopts work

### [](#week-08)week 08

- Midterm exam

### [](#week-09)week 09

- `sftp`
- review
- assignment 2

#### Things you should know:

- What is sftp

### [](#week-10)week 10

- systemd
- `systemctl`
- services

#### Things you should know:

- What is a service manager
- pid 1
- general understanding of .timer and .services 
- general systemctl commands
    - start
    - stop
    - reload
    - enable
    - disable
    - status
    - mask
    - unmask
- Where are service files, what is in a service file?
    - oneshot vs simple
        - simple is the default 
            - when you want a service to be maintained. you want it to continue running
        - oneshot just run once and then they're done 
    - ExecStart
    - wants vs requires
        - wants is a weak or optional dependency
        - requires is a necessary dependency 

### [](#week-11)week 11

- pacman
- systemd timers

#### Things you should know:

- general package manager, what and why
- pacman commands
    - install
    - remove
    - search
    - update

### [](#week-12)week 12

- nginx
- ufw
- assignment 3

#### Things you should know:

- nginx configs used in the class
- general nginx info
- test an nginx config

### [](#week-13)week 13

- nginx
- reverse proxy
- load balancing

#### Things you should know:

- General, what is a load balancer and what is it good for
- ports
    - 80
    - 22