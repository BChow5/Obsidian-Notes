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

##### What are ssh keys, where are they used and why
- SSH = Secure Shell Keys
- SSH keys are a pair of keys used for secure authentication
	- public key - shared freely and stored on the server
	- private key - kept secure by client
	- private key is used to prove the client's identity and the public key verifies the identity 
- SSH keys are used for:
	- remote server access
	- secure file transfers 
	- version control systems (e.g. github)
	- automation
- Why are SSH keys used? 
	- more secure than a password
	- two factor mechanism
	- more convenient since you don't need to login with password 

##### Where are ssh keys on your local machine, what about on the server
- **Server:** public keys stored at `~/.ssh/authorized_keys`
- **Local Machine**: Private and public keys are in `~/users/<username>/.ssh`

##### General method of communication
- what is happening when you make an SSH connection?
	1. **Client Initiates Connection**
	2. **Server Sends Identification**
	3. **Key Exchange and Session Establishment**
	4. **Authentication Phase**
	5. **Connection Established**

##### SSH command
-` ssh -i `is to specify identity file (private key) to use for ssh
  
##### ssh-keygen, creating ssh keys
- how do we create keys?
	- ``ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your email address"``
		- `-t` - Specifies the type of key to create. Encryption method
		- `-f` - specifies the file name and path where key is saved
		- `-C` add a comment to the public key
- where do keys go?
	- **server:** `~/.ssh/authorized_keys`
	- **local:** `~/users/<username>/.ssh`

##### Basic vim movements

| Action                            | Vim Command | Description                |
| --------------------------------- | ----------- | -------------------------- |
| Move Cursor                       | h           | move cursor left           |
|                                   | j           | move cursor down           |
|                                   | k           | move cursor up             |
|                                   | l           | move cursor right          |
| move to start of line             | 0           |                            |
| move to end of line               | $           |                            |
| move to top of buffer             | gg          | move to top of document    |
| move to bottom of buffer          | G           | move to bottom of document |
| delete line                       | dd          | deletes current line       |
| delete multiple lines             | n dd        |                            |
| delete from cursor to end of line | D           |                            |
| search for a term                 | /           |                            |

##### Vim Commands
- modes
	- `esc key` - normal mode
	- `i` - insert mode
	- `v` - visual mode
	- `:` - command line mode
- commands
    - `:q` quit
    - `:q!` quit without saving
    - `:w` save file
    - `:wq` save and quit
    - `:x` exit if no changes

### [](#week-03)week 03

- Linux file system
- cloud-init

#### Things you should know:

##### frequently used directories and files
- `/etc`
	- System configuration files. These files control system-wide settings.
	- `/etc/passwd`
		- Contains user account information (e.g., usernames, UID, GID, home directory, etc.)
	- `/etc/shadow`
		- contains hashed passwords for users
		- only readable by root 
- `/usr`
	- contains user related programs and data that is shareable across systems 
	- `/usr/bin`
		- contains binaries (programs) that are available for all users on the system
	- `/usr/lib` libraries and other object files required for system and application binaries 
- `/bin`
	- contains essential system binaries required for basic operations
	- these are needed even when the system is in a minimal state (e.g. rescue mode)
- `/var`
	- stores variable data that can grow in size
	- e.g. log files, run time data
- home directory
	- **regular users**: `/home/user`
	- **root**: `/root`
- check `man hier`
	- general info about the hierarchy 
##### file system hierarchy general info

##### what is cloud-init, why is it useful
- cloud-init is a tool for automating the initlaization of cloud instances (virtual machines) on their first boot
	- auto configuration for setting up users, installing packages, running scripts, and configuring networks
- cloud-init supports configuration in **YAML** format 
- how does it work?
	- Initialization
		- When a cloud instance is created, cloud-init starts automatically during the first boot sequence. 
		- It retrieves configuration data from metadata sources or user-specified cloud-config files.
	- Customization
		- Set up network configurations
		- Create users and set passwords
		- Install and configure software packages
		- Execute custom scripts or commands
	- execution
		- cloud-config files
			- used to define the instance’s configuration and are specified at instance launch

**Why is it useful?**
- automation
	- automates user creation, software installation, environment configuration
	- important for scaling applications
- consistency
	- ensures each instance is initialized in the same way
- flexibility

### [](#week-04)week 04

- processes
- ps
- find
- grep

#### Things you should know:

##### What is a process?
- a process is an instance of a program that is being executed

##### How are commands run in the shell
- what is the process of running a command?

##### ps
- `ps` command is process status. 
    - display information about the currently running processes on the system
    - `-e` - shows all processes 
    - `-o` - customize the output of the `ps` command by specifying which fields (or columns) to display

##### general find and grep commands

**Find**
- locate files or directories based on name, type, size, etc.
- `find <path> -name <filename>`
- `-name` searches for a file by name
- `-user` searches for a file owned by a user 
- `-mtime` file modified by time 
- `-r` recursive search
- `-type` specifies the type of file you want to search for
	- `f` file
	- `d` directory
	- `l` symbolic link 
- examples
	- `find ~ -type f` find all of the regular files in your home directory
	- `find . -type f -name "*.txt"` find all files in the current directory, or sub directory that end in ".txt".
	- `find . -type f -mmin -10` find all of the files in the current directory that were modified in the last 10 minutes (less than 10 minutes ago)
	- `find . -type f -mmin +10` same as above, but will find files modified more than 10 minutes ago.
	- `find / -size +10M` find files larger than 10 Megabytes anywhere in the root directory or sub-directories.

**Grep**
- used to search for patterns within files.
- `grep <pattern> <filename>`
- `-i` ignore case
- `-r` recursive
- `-n` display line number where pattern matches
- `-w` exact match
- `-c` count the number of matching lines
- `grep -C <num> "pattern" file` The number of lines of context to display before and after the matching line.

- examples
	- `grep search-word file` basic grep command
	- `grep -iw search-word file` search only for the search word, not sub-strings, and ignore case
	- `grep -r search-word directory` search recursively for all instances in a file. -r and -R are slightly different, see the grep man page for more
	- `grep -C 2 "search words" file` return found instances of a search term with 2 lines above and below where the search pattern was found, for context.
	- `grep -c "set" ~/.bashrc` count the number of times "set" is found in our .bashrc file
	- `grep -rl "search pattern" dir` return a list of files that contain the search pattern
	- `fc-list | grep -i "nerd"` find all of the nerd fonts you have installed on your system uses a pipe to filter the output of the `fc-list` command.

##### Regular Expressions
- `grep "[0-9]{2}" file` find all lines in file that contain a number 0-9 in two consecutive digits 
- `grep -P '\(\d{3}\) \d{3}-\d{4}|\d{3}-\d{3}-\d{4}' file` This will match phone numbers written as: (555) 555-5555 or 555-555-5555 in a regular expression '|' is or

### [](#week-05)week 05

- users
- groups
- permissions
- Neovim config example

#### Things you should know:

##### relationships between users groups and permissions
- only 1 user and 1 group can own a file 
**Users**
- 1 user can be in any number of groups
- unique User ID (UID)
- user default home directory - /home/user
- UID 0 is root 
- two types of users:
	- **Regular users** - normal user with limited permissions
		- UID 1000+
	- **System users** - users created for running system processes  
		- UID 0-999
	- UID 65534 Is user `nobody`

**Groups**
- a collection of users, used to simplify and manage permissions collectively
- has a unique Group ID (GID)
- **Primary Group**: The default group assigned to a user.
- **Secondary Group**: Additional groups a user belongs to, granting them extra permissions.

##### Where is user info stored?
- `/etc/passwd`
	- contains info on:
		- username
		- password
		- UID
		- GID
		- Name
		- location of home directory
		- location of login shell 
- `/etc/shadow`
	- The shadow file stores users encrypted passwords as well as information about the users password
	- login name
	- encrypted password
	- days since Jan 1 1970 that password was last changed
	- days before password may be changed 
	- days after which password must be changed
	- days before password is to expire that user is warned
	- days after password expires that account is disabled
	- days since Jan 1, 1970 account iis disabled 
##### Commands for managing users and groups
- `chmod` change the **permissions** of a file or directory
	- e.g. `chmod [options] <permissions> <file/directory>`
- `chown` changes the **owner** and **group** of a file or directory
	- e.g. `chown [options] <new-owner>:<new-group> <file/directory>`
	- `-R` recursive
- `useradd` create a **new user** on the system
	- e.g. `useradd [options] <username>`
	- `-m` create the user's home directory
	- `-G` add user to specific groups
	- `-s` specify the default shell for user 
- `usermod` **modify** an existing user
	- e.g. `usermod [options] <username>`
	- `-aG` add user to additional groups
	- `-L` lock the user account
	- `-U` unlock the user account 
- `userdel` deletes an existing user from the system
	-  e.g. `userdel [options] <username>`
		- `-r` removes the user's home directory 
- `groups` shows the groups that a user belongs to
- `groupadd `used to create a **new group** on the system
	- e.g. `groupadd [options] <groupname>`
	- `-g` specify the group ID
- `passwd` prompts to create a password
	- e.g. `passwd <user-name>`
	- `-l` to lock a user account
	- `-u` unlock an account

##### Sudo
- allows a user to temporarily perform tasks with elevated privileges
- sudo group or wheel group for sudo privileges

| File            | Purpose                                               |
| --------------- | ----------------------------------------------------- |
| /etc/sudo.conf  | Configuration for the sudo command                    |
| /etc/sudoers    | Configuration for who can use sudo                    |
| /etc/sudoers.d/ | A directory containing individual configuration files |

##### Permissions and Ownership
- `r` = read
- `w` = write
- `x` = execute
- `=` a place holder for permissions the user does not have

|Pattern|Effective permission|Decimal representation|
|---|---|---|
|---|None|0|
|--x|Execute|1|
|-w-|Write|2|
|-wx|Write and execute|3|
|r--|Read|4|
|r-x|Read and execute|5|
|rw-|Read and write|6|
|rwx|Read, write, execute|7|

##### Where is your Neovim config
`/home/<user-name>/.config/nvim/init.lua`

### [](#week-06)week 06

- intro to bash scripting
- variables
- conditionals
	- e.g. be able to check if a file or directory exists
- loops
	- e.g. loop through an array

#### Things you should know:

##### Shebang
- first line of a script shell 
- tells the shell which interpreter to use when the script is executed 
- `#!/bin/bash` or `#!/usr/bin/env python`
- more convenient so we don't need to specify the shell whenever we run the script

##### Quotes in bash
- **Single quotes** - used for literal string
	- doesn't do expansions of variables
- **Double quotes** - allows variable expansion, command substitution

#### Variables in bash
- **variable** - `animal="wolf"`
- **environment variable** `export variable=value`

##### Positional Parameters 
| Special Variable | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `$0`             | The name of the bash script.                         |
| `$1,$2...`       | The bash script arguments.                           |
| `$$`             | The process id of the current shell.                 |
| `$#`             | The total number of arguments passed to the script.  |
| `$@`             | The value of all the arguments passed to the script. |
| `$?`             | The exit status of the last executed command.        |
| `$!`             | The process id of the last executed command.         |
| `$*`             | All arguments treated as a single string             |
|                  |                                                      |

##### Conditionals
an if statement in bash is written like:

```bash
if [[ test ]]; then
	consequent
fi
```

Bash also includes `elif` and `else` conditions.

```bash
if [[ test ]]; then
	consequent
elif [[ second test ]]; then
	consequent
else
	consequent
fi
```

##### Arrays
Creating an indexed array

```bash
declare -a arr=("1" "2" "3")
# or
arr=("1" "2" "3")
# Array items are separated by whitespace
```

Accessing array values

```bash
#!/usr/bin/env bash
arr=("1" "two" "3")
# access by index
echo "${arr[0]}"
# print all of the array items
echo "${arr[@]}"
# print number of array items
echo "${#arr[@]}"
```

Creating an associative array

```bash
declare -A person
person["name"]="Jan"
person["age"]="31"
```

Accessing values and keys in an associative array

```bash
# print all values
echo "${ex_arr[@]}"
# print all keys
echo "${!ex_arr[@]}"
# print value by key
echo "${ex_arr["key1"]}"
```

##### Loops
The basic syntax of a for loop is:

```bash
for item in [LIST]; do
  [COMMANDS]
done
```

You can also write c style for loops

```bash
for ((i = 0 ; i <= 1000 ; i++)); do
  echo "Counter: $i"
done
```

while loop
```bash
while [ condition ]; do
    # Commands to execute
done
```

- **Break**
	- Exit from a `for`, `while`, `until`, or `select` loop.
- **Continue**
	- Resume the next iteration of an enclosing `for`, `while`, `until`, or `select` loop.

##### Loop through an array
```bash
for element in "${my_array[@]}"; do 
	echo "$element" 
done
```

##### Command Substitution 
- `$(command)`
- e.g. `info=$(find . -name "$file")`

##### why var=val works vs var = val
- with spaces doesn't work because the shell sees this as a command with **three parts**: `var`, `=`, and `value`

##### write a conditional that will check if a file or directory exists
```bash
if [[ -f "filename" ]]; then 
	echo "File exists."
fi
```

```bash
if [[ -d "directoryname" ]]; then 
	echo "Directory exists."
fi
```

##### When you run a command where does the shell search for that command, in what order?
1. shell function 
2. shell built ins
3. commands in the file path

##### bash configuration files where and when
- `/etc/bash.bashrc`
- `~/.bashrc`
### [](#week-07)week 07

- bash expansions
- bash functions
- bash case statements
- handling options with getopts

#### Things you should know:

##### What is an expansion in bash
- brace expansion 
	- `echo {1..5..2}` returns 1 3 5
- tilde expansion
	- `~` -> `/home/user-name`
- command substitution
	- `$(command)`
- variable expansion
	- `$variable`
- arithmetic expansion
	- `$(( expression ))`
- filename expansion 
	- `*` matches any sequences of character
	- `?` matches any single character
	- `[]` matches any enclosed characters

##### ${var} vs $var
- **`$var`**: Used for simple variable expansion, where the variable is standalone.
- **`${var}`**: Used when the variable is part of a more complex expression (e.g., within strings or array indexing), or when you want to avoid ambiguity in variable names.

##### function arguments and returns, general function syntax

How to declare a function
```bash
function_name() { 
# commands 
}
```

```bash
function function_name { 
# commands 
}
```

**Function Arguments**
- they use positional parameters for functional arguments 
```bash
greet() {
  echo "Hello, $1! Welcome to $2."
}

# Call the function with arguments
greet "Alice" "Bash"  # Output: Hello, Alice! Welcome to Bash.
```

**Local Variables**

```bash
localretrn () {
  local func_result="some result"
  echo "$func_result"
}
```

##### Return a value from a bash function
```bash
function add {
    echo $(( $1 + $2 )) # print the value with echo
}
result=$(add 3 5) # capture the value printed in a new variable
echo "The result is: $result"
```

##### Heredoc
```bash
cat > newfile <<- EOF
	Line one
	Line two
EOF
```

##### general case statement syntax
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

```
) indicates the test, ie the pattern to try to match

;; indicates the end the condition

* Matches any string of characters. Use for the default case.

? Matches any single character
 
You can use a range [a-z] in a case statement.

| Separates alternative choices that satisfy a particular branch of the case structure.
```

##### what is getopts, general how does getopts work
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

##### OPTARG
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

##### `shift` positional parameters
```bash
#!/bin/bash
echo $1 $2 $3 # prints 1 2 3
shift
echo $1 $2 $3 # prints 2 3 4
shift 2
echo $1 $2 $3 # prints 4, it shifts 2 additional positions
```

##### OPTIND
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
### [](#week-08)week 08

- Midterm exam

### [](#week-09)week 09

- `sftp`
- review
- assignment 2

#### Things you should know:

##### What is sftp
- SFTP, the Secure(or SSH) File Transfer Protocol
- method of transferring files to and from remote servers
- uses SSH for security 
- `get` to download a file from the remote server
- `put` to upload a file 

### [](#week-10)week 10

- systemd
- `systemctl`
- services

#### Things you should know:

##### Unit files
- A unit file is a plain text ini-style file that encodes information about a service, a socket, a device, a mount point, an automount point, a swap file or partition, a start-up target, a watched file system path, a timer controlled and supervised by [systemd](https://man.archlinux.org/man/systemd.1.en)

##### Locations of unit files

| Directory                  | Description                                                                                                                                                                       |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/usr/lib/systemd/system/` | Systemd unit files distributed with installed Pacman packages.                                                                                                                    |
| `/run/systemd/system/`     | Systemd unit files created at run time. This directory takes precedence over the directory with installed service unit files.                                                     |
| `/etc/systemd/system/`     | Systemd unit files created by `systemctl enable` as well as unit files added for extending a service. This directory takes precedence over the directory with runtime unit files. |

##### What is a service manager
- systemd is a system and service manager
- acts as initialization system that brings up and maintains user space services 
- **PID 1 = systemd**
	- systemd provides several tools that perform general tasks required by an operating system

##### general understanding of .timer and .services
**Services**
- what systemd refers to daemons as
- a service is any "background" process, running without a user interface
- generally waits for events to occur that trigger some response
	- e.g. web server and sshd

**Targets**
- a group of services that are attached through dependencies that start all of the services required to meet some system state
- e.g. multi-user.target, which is when all of the services needed for a minimal working server have started
- see current targets with `systemctl list-units --type=target`

**Timer**
- a unit file used to define timer targets that trigger actions at specific times or interval 

##### general systemctl commands
- start
	- `sudo systemctl start <unit>`
- stop
	- `sudo systemctl stop <unit>`
- reload
	- `sudo systemctl reload <unit>`
- reload systemd manager
	- `sudo systemctl daemon-reload`
- enable
	- `sudo systemctl enable <unit>`
	- start automatically at boot
- enable and start
	- `sudo systemctl enable --now <unit>`
- disable
	- `sudo systemctl disable <unit>`
- status
	- `sudo systemctl status <unit>`
- mask
	- `sudo systemctl mask <unit>`
	- makes it impossible to start
- unmask
	- `sudo systemctl unmask <unit>`

##### Where are service files, what is in a service file?
- `.service` are unit files that defines a service that systemd will manage 
- typically has three main sections of
	- `[Unit]`
		- contains meta data about the service like description and dependencies
	- `[Service]`
		- defines behaviour of the service itself
		- including how its started, stopped, and restarted
	- `[Install]`
		- defines installation related settings and how the service should be enabled or linked into the system
- system wide service files at `/lib/systemd/system` or `usr/lib/systemd/system`
- custom service files at `/etc/systemd/system`

**oneshot vs simple**
- goes in `[service]`
- `Type=simple` used for services that run in the foreground and don't exit quickly
	- simple is the default
	- when you want a service to be maintained and you want it to continue running
- `Type=oneshot`
	- runs once and then they're done

**ExecStart**
- defines the command that should be executed when starting the service 
- specifies the path to the program or script that will run when service is started
- `ExecStart=/path/to/command [arguments]`

**wants vs requires**
- wants is a weak or optional dependency
- requires is a necessary dependency 

### [](#week-11)week 11

- pacman
- systemd timers

#### Things you should know:

##### general package manager, what and why
- a package manager is a tool that makes it easier for a user to manage software on their system
- allows you to perform operations like installing, removing, and updating software 
- benefit is they handle dependencies 
- ArchLinux uses pacman as its package manager 

##### pacman  and commands
 This list below is a good place to start for day to day use, but doesn't include every `pacman` command.

| command                    | Action                                                         |
| -------------------------- | -------------------------------------------------------------- |
| `pacman -Sy`               | Refresh the local package repository                           |
| `pacman -Syu`              | Upgrade installed packages                                     |
| `pacman -S <packages>`     | Install one or more packages and any dependencies              |
| `pacman -Ss <search term>` | Search for a package                                           |
| `pacman -Rns <packages>`   | Remove a package, configuration files and package dependencies |
| `pacman -Si <package>`     | Display remote information about a package                     |
- configuration for pacman is in `/etc/pacman.conf` the mirrors list is in `/etc/pacman/pacman.d/mirrorlist`

##### Systemd timers
- a timer is a type of unit file that allows you to start a service at a specified time 
- **monotonic** timers
	- start in relation to other system events such as on boot
```
[Unit]
Description=Run foo weekly and on boot

[Timer]
OnBootSec=15min
OnUnitActiveSec=20d

[Install]
WantedBy=timers.target
```

- **realtime** timer
	- start on calendar events
```
[Unit]
Description=Run foo weekly

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```
- e.g. to run it every day of the week `OnCalendar=dayoftheweek*-*-* 04:00:00`
	- year, month, day for the asterisks 


### [](#week-12)week 12

- nginx
- ufw
- assignment 3

#### Things you should know:

##### general nginx info
- nginx is a web server
	- a webserver is software that we can run on our hardware server 
	- serves documents over the internet 
- nginx made by Igor Sysoev

##### nginx configs used in the class
- main nginx configuration in `/etc/nginx/nginx.conf`
- two methods for creating a server block
	- in `nginx.conf`
	- or a file in a directory `sites-available`
		- then you create a symbolic link of that file to `sites-enabled`


example server block configuration
```
server {
    listen 80;
    listen [::]:80;
    
    server_name domainname1.tld;
    
    root /usr/share/nginx/domainname1.tld/html;
    index index.html;

	location / {
        try_files $uri $uri/ =404;
    }
}
```
- listen 80 means to listen for HTTP on port 80
	- **`listen [::]:80;`** for ipv6 
- port 22 is SSH
- **`root /usr/share/nginx/domainname1.tld/html;`**:
	- sets the root directory for serving content
- `index index.html`
	- defines default file to serve when a directory is requested 

##### test an nginx config
- ``sudo nginx -t``

### [](#week-13)week 13

- nginx
- reverse proxy
- load balancing

#### Things you should know:

##### General, what is a load balancer and what is it good for
- a load balance is a system used to ensure high availability by distributing traffic to two or more servers 
- benefits of increased availability, resilience, and scalability 
	- failover: when a server fails, the traffic will be rerouted to another available server 

##### Deployment strategies
- phased deployment
	- e.g. canary deployment
- big bang deployment
	- deploying new version all at once 

##### Root vs alias
**Root**
- The path in location is appended to the root path
```
location /files {
  root /var/www/html
}
```
- looks at /var/www/html/files
- appends url to file path

**Alias**
```
location /files {
  alias /var/www/html/
}  
```
- does not append the url to the file apth
##### ports
- 80 - http
- 22 - ssh