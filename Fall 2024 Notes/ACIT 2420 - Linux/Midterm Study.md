## Lecture 2 - SSH and Shell
### SSH
- <mark style="background: #ABF7F7A6;">Secure Shell Protocol (SSH)</mark> is a method for securely sending commands to a computer over an unsecured network 
- SSH uses cryptography to authenticate and encrypt connections
	- uses a public and private key
	- public key available for anyone to use and private key that is secret only to the owner
	- the two keys establish the owners identity

#### What is SSH used for?
- remotely managing servers, infrastructure, and employee computers
- securely transferring files
- accessing services in the cloud without exposing a local machine's port to the internet
- connecting remotely to services in a private network
- bypass firewall restrictions 

#### Making an SSH key
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### The Shell and Commands
- <mark style="background: #ABF7F7A6;">Shell</mark> makes an operating systems system's services accessible to users or other programs
- fundamental capability of shells is the ability to launch command line programs that are already installed on the system
- Linux users can use a variety of shells 
- the Shell functions as a REPL
	- read, evaluate, print, loop
- **When you run a command the shell looks for that command in several locations:**
	- Shell built-ins
	- Shell functions
	- Commands in your file path

#### Built in Shell Commands
- when you type a command into the terminal, the shell first checks if it is a command that is built into the shell itself
- if not, then it will search through a list of folders (called the search path) to find the command 
- `$PATH` is an environment variable that stores a list of directories the shell searches for executables.

#### Basic Shell Commands
- `pwd` - print working directory 
	- pwd command determines which directory you are in
- `cd` - change directory
- `mkdir`
	- mkdir command to make a new directory 
- `rmdir`
	- deletes a directory
	- can only be used to remove empty directories
	- To remove files and directories, use **rm -rf directoryname/** (where **–rf** will recursively remove all the files and directories from inside the directory).
- `touch`
	- used to make empty files
- `ls`
	- ls command to see all files and directories inside the directory you're in
	- ls `-a` command to see hidden files
	- `-l`list files and directories in a detailed (long) format
- `cp`
	- copy files and directories from the command line 
	- requires two arguments
		- first specifies the specific location of the file
		- second specifies where to copy
	- e.g. cp file1.txt testfolder/
	- `-p` to preserve file permissions and timestamps
	- need to use `-r` for recursive when copying a directory 
	- `-f` to forcefully do it
	- `-v` verbose to display details 
- `mv`
	- mv command to move a file or directory from one location to another or even to rename a file 
	- e.g. - **mv file1.txt file2.txt**
	- `-f` to forcefully do it 
	- `-v` verbose to display details
- `rm`
	- remove files or directories 
		- use -r or -f to recursively remove a directory (-r) or force remove a file or directory (-f)
- `locate`
	- used to find the location of a file
	- -i argument helps to ignore case sensitivity 

#### Intermediate Shell Commands
- `echo`
	- allows you to **display a line of text, variable or string** to standard output
- `cat`
	- cat command is used to read the content of a file
	- you can use cat command and append the output of a new file using `>>`
- `df`
	- displays disk space usage on file systems
	- lets you see the overall disk size, amount of space used, and the amount of space used, the amount of space available, the utilization percentage, and the partition that the disk is mounted on
	- use `-h` parameter to make the data legible by humans
- `du`
	- shows the size of a specific directory or subdirectory
- `uname`
	- displays info regarding the operating system that your Linux is on
- `chmod`
	- system call and command used to modify the special mode flags and access permissions of file system objects 
	- can change permissions for read, write, and execute 
	- The symbolic permissions notation is used in this example. **u**, **g**, and **o** stand for _user_, _group_, and _other_, respectively. The letters **r**, **w**, and **x** stand for _read_, _write_, and _execute_,
	- **4** stands for read, Write has the prefix **2** ,**1**  denotes _execute_ ,**0** means _no authorization_ 
		- **7** is made up of the permissions **4**+**2**+**1** (read, write, and execute), **5** (read, no write, and execute), and **4** (read, no write, and no execute).
- `chown`
	- change owner of the system files and directories on Unix
- `chgrp`
	- a filesystem object's group can be changed to another one 

### Navigating the Shell
- a users home directory is `/home/user-name`. Two shortcuts for this are:
	- `~`
	- `$HOME` the home path environment variable
	- #midtermQ
- the `ls` command is to list the contents of a directory has a few options
	- `-l` long listing  (detailed list)
	- `-a` all (shows hidden files)
	- `-F` sorts and adds additional file type classifications

## Lecture 3 - File System and Cloud-init

### Linux File System
- a filesystem is the method and data structure that an OS uses to keep track of files on a disk or partition
	- the way files are organized on the disk
- ![[Pasted image 20240915110507.png]]
  - most Linux distros have files and directories organized according to the <mark style="background: #ABF7F7A6;">Filesystem Hierarchy Standard </mark>
  - **the filesystem begins at the root directory `/` and branches out like a tree**

#### Some Directories 
- `/` the root
	- the root directory is the start of the tree
	- inside the root directory are key directories used to store specific files on your system
- `/boot`
	- boot contains static files for the boot loader
	- stores data that is used before the kernel begins executing user-mode programs
- `/dev`
	- contains special or device files for physical devices 
- `/usr` 
	- holds the bulk of user-space applications, libraries, and documentation, but it doesn't store personal user files. Instead, it contains programs and data shared by users.
- `/etc`
	- contains configuration files
	- `/etc/passwd` stores information about user accounts **but not passwords**
	- `/etc/shadow` stores hashed passwords  and password information
- `/bin` -> `/usr/bin`
	- essential user command binaries
	- historically stored utilities needed to run in single user mode
	- now bin is generally a symbolic link that points to /usr/bin
	- `/usr/bin` contains binary executable files that are available to all users on the system 
- `/sbin`
	- essential system binaries 
- `/proc` 
	- contains (among other things) one subdirectory for each process running on the system
		- which is named after the process ID (PID)
	- proc is a pseudo filesystem 
		- a pseudo file system is a way to work with non-file data as though it were a file 
- `/var`
	- contains variable data files
- `/home` 
	- regular users home directories are stores here
		- e.g. a user named Kim would have their home directory in `/home/kim` #midtermQ 
- `/root` 
	- the home directory for the root user 

### Symbolic Links
- a <mark style="background: #ABF7F7A6;">hard link</mark> is a pointer to a file (the directory entry point to the inode)
	- A hard link will continue to point to the original file even if the file’s name is changed, unlike a soft link
	- As opposed to symbolic links, hard links prevent files from being deleted or moved
- <mark style="background: #ABF7F7A6;">inode</mark> is a data structure used to store information about files and directories
	- every file and directory has an associated inode that contains meta data about the file **but not the file's name or actual data**
- <mark style="background: #ABF7F7A6;">symbolic link</mark> is an indirect pointer to a file (the directory entry contains the pathname of the pointed to a file)
	- essentially shortcuts that refer to a file rather than the inode value of the file they point to
- advantage of symbolic links
	- you can't create a hard link to a directory, but you can create a symbolic link to a directory
- dereferencing symbolic links
	- when you dereference a symbolic link, you follow the link to the target file
- creating a symbolic link
	- create with the `ln` utility using the `-s` option
```bash
`ln -s [OPTIONS] FILE LINK`
```

```bash
`ln -s source_file symbolic_link`
```

### Cloud-init
- a tool used to initialize and configure cloud instances automatically 
	- helps to automate the deployment of cloud instances
- can be used to configure settings on new virtual machines  like 
	- hostname
	- network settings
	- create users and groups
	- install packages
	- run commands
	- add SSH keys
- YAML files 
	- a file format used for data serialization that is both human-readable and machine-readable. YAML stands for **YAML Ain't Markup Language**, emphasizing its purpose as a data serialization format rather than a markup language.
- If the configuration in your file is not being applied you probably won't see an error message. You can find one in the logs (`journalctl -b`)

```bash
#cloud-config
users:
  - name: user-name #change me
    primary_group: group-name #change me
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519 ...

packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux

disable_root: true
```

## Lecture 4 - Linux Processes and Finding Files

### The Linux Boot Process 
- UEFI/BIOS
	- power on self test (POST)
		- checking your hardware before startup
- Boot Loader
	- GRUB2, systemd-boot
		- systemd is lighter and faster
		- GRUB2 has more options but slower
- kernel 
- init
	- systemd, runit, r6, openRC
	- started by kernel

### What is a Process?
- an instance of a running program
- Every time a program is executed, the operating system creates a process to run that program

### The Process Tree
- everything you launch from your shell becomes a child process of that shell
- the shell itself is also a child process
- all processes have a parent and all running process relationships form a tree with a single root (PID = 1)
	- the PID =1 can be anything
- the `init` process is the only process that is launched directly by the kernel
- you can view the full process tree with the `pstree` command
	- good to visualize all running processes and relationships between them 
	- but in practice most admins look for specific processes or need to learn about their resource usage rather than just their existence

### `ps` utility
- `ps` allows you to take a snapshot of running processes
	- PS stands for process snapshot
- utility to allow you to retrieve and filter info about running processes 
- `-a` option display a list of processes running in the terminal for all users, excluding the session leaders (the initial shell processes).
- `-x` option that are detached from a terminal
- ![[Pasted image 20240923215856.png]]
	- `PID` is the process identifier
	- `TTY` is a terminal
	- `TIME` the accumulated CPU time the process has used.
	- `CMD` shows the command that was used to launch a process along with its arguments if any were used 
	- `STAT` tells us the process state 
	- `%MEM` percentage of RAM the process is using 
- options for `ps`
	- `-e` displays everything
	- `aux` shows detailed list with info about users, CPU usage, memory
	- `-f` full format
	- `-u` filter by user
	- `-H` displays procsses in a heirarchical structure 
	- `-- forest` display the running processes in a **tree-like, hierarchical structure**
#### Process Monitoring Tools
- could use `top` to check how much RAM a process is using
	- it displays an interactive process list, with processes that consume the largest resources automatically floating to the top
	- good if you're trying to find out what is causing CPU usage spikes 
- `htop` offers different user interfaces and additional functionality
- `iotop` tool to monitor the input/output activity of processes  

### Executable and Linkable Format (ELF)
- default executable file format on Linux
- ELF files store executable code of programs
	- can also store debug info
- ELF files can also be linked with other ELF files
	- known as **shared libraries**
- stores meta data such at target OS and CPU architecture 

### `/proc` filesystem
- lowest level interface to process info
	- an example of a virtual filesystem
- contains runtime system information that the kernel generates on the fly
- a virtual filesystem that provides a mechanism for the kernel to communicate with user space
- `ps` and `pstree` both use `/proc` to get their info 

- **/proc/cpuinfo**: Information about the CPU
- **/proc/meminfo**: Information about the physical memory
- **/proc/vmstats**: Information about the virtual memory
- **/proc/mounts**: Information about the mounts
- **/proc/filesystems**: Information about filesystems that have been compiled into the kernel and whose kernel modules are currently loaded
- **/proc/uptime**: This shows the current system uptime
- **/proc/cmdline**: The kernel command line

### Exit Codes or Exit Status
- when a process terminates for any reason it returns an exit code
- you can view the exit code of the last command with `echo $?`
- exit code 0 indicates success
	- the command did what it was told to do
		- this isn't the same as what you want it to do
- none 0 indicates an error

### Signals
- signals provide a way for a user or another process to communicate with a running process
- `SIGINT` is the signal sent when you process `ctrl +c`
	- attempts to interrupt a foreground process
- `SIGTERM`
	- attempts to terminate a process
		- graceful shutdown
	- allows a process to finish what it is doing first
- `SIGKILL`
	- forces a process to quit immediately 
- `SIGILL`
	- illegal instruction 
	- e.g. attempting to divide by zero
- `SIGSEV` 
	- segmentation violation 
	- e.g. caused by trying to read or modify memory that wasn't allocated to the process 
- `SIGPIPE` 
	- generated when a network socket or local pipe is closed by the other end 

### Terminate process with `kill` and `pkill`
- `kill` is a shell builtin command
- `pkill` is easier to use because you can use pattern matching for the process name instead of using PID
	- e.g. `pkill Chrome` would work to end Chrome
- `kill` is the command for sending signals to stop process
	- used to forcibly terminate process but it can also send other signals
- by default, `kill` command sends a `SIGTERM` signal
- `pkill` can use a pattern match for a process name 
- you can specify what kind of signal you want to send 
	- e.g. `pkill -9 <process_name>`
		- `-9` is the `SIGKILL` signal

### Finding files with `find` and `grep`

#### Find files using `find`
- `find` utility can be used to find files based on file properties
	- file name `-name`
	- type `-type`
	- size `-size`
	- when last modified `-mtime`
	- etc
- types
	- `-f` file
	- `-d` directory
	- `l` symbolic link
- ![[Pasted image 20240924232550.png]]

##### examples

`find ~ -type f` find all of the regular files in your home directory

`find . -type f -name "*.txt"` find all files in the current directory, or sub directory that end in ".txt".

`find . -type f -mmin -10` find all of the files in the current directory that were modified in the last 10 minutes (less than 10 minutes ago)

`find . -type f -mmin +10` same as above, but will find files modified more than 10 minutes ago.

`find / -size +10M` find files larger than 10 Megabytes anywhere in the root directory or sub-directories.

#### -exec

`-exec <command> {} \;` use this to execute a command on files found by find `-exec <command> {} +` same as above, but collect files, then run command. This option will involve fewer invocations of the command.

#### Find files with `grep`
- `grep` utility can be used to find patterns of text in a file
```bash
`grep [_OPTION_...] _PATTERNS_ [_FILE_...]`
```
![[Pasted image 20241001215200.png]]
- `pipe`
	- `|` is the pipe symbol
	- allows  the output of one command to be used as the input to another command
	- enables chaining of commands to perform complex operations 

| option | meaning                                                                                                              |
| ------ | -------------------------------------------------------------------------------------------------------------------- |
| -r     | recursive - search whole directory tree (don't follow links)                                                         |
| -R     | recursive - search whole directory tree (follow links)                                                               |
| -i     | ignore case of text                                                                                                  |
| -n     | print line numbers                                                                                                   |
| -v     | invert match - print lines that do not match                                                                         |
| -c     | count the number of matching lines                                                                                   |
| -rl    | recursively search for files that contain a specific pattern and list the **file names** (not the matching lines)    |
| -l     | tells `grep` to only **list the names of the files** that contain matches, instead of displaying the matching lines. |
![[Pasted image 20241001215420.png]]

#### `grep` and `find` together
![[Pasted image 20241001215537.png]]
### File Globbing

#### Wildcards

| Wildcard | matches                                                       | example       | explanation                  |
| -------- | ------------------------------------------------------------- | ------------- | ---------------------------- |
| ?        | a single character                                            | ls b?b.txt    | matches bob.txt and bab.txt  |
| *        | zero or more characters                                       | ls bo*.txt    | matches bob.txt and both.txt |
| [ ]      | letters inside brackets                                       | ls [bs]ob.txt | matches bob.txt and sob.txt  |
| ^        | asserts that the match must occur at the start of the string. |               |                              |
- brackets can also contain ranges
	- e.g. 
		- [a-z] matches lower case alphanumeric letters 
		- [0-9] matches numbers 0 through 9
		- [0-9t] matches numbers 0 through 9 and the letter "t"

#### regular expressions

##### examples
`grep -P "\d{2}" file` this uses the -P option which tells grep to use a perl compatible regular expression. Not all versions of grep have this option.

`grep "[0-9]{2}" file` will do the same as above

`grep -P '\(\d{3}\) \d{3}-\d{4}|\d{3}-\d{3}-\d{4}' file` This will match phone numbers written as: (555) 555-5555 or 555-555-5555 in a regular expression '|' is or

## Lecture 5 - Permissions, Users, and Groups
- a <mark style="background: #ABF7F7A6;">container</mark> is a lightweight, isolated environment that allows you to run applications and their dependencies independent of the host system 
	- Containers package everything the software needs to run, including the code, runtime, system tools, libraries, and settings
	 - less resource intensive that a virtual machine
- **general/normal users** and **root/superusers** are the two categories of users in Linux systems 
- adding new users requires specific permissions (superuser)

### User Groups and Permissions 
- e**very file belongs to a user and a group**
	- just one of each
	- reminder that **everything is a file** 
- a user can belong to multiple groups 
- files have permissions for:
	- the **user** that owns the file
	- the **group** that owns the file
	- **other** (everyone else)
- the permissions are 
	- read
	- write
	- execute

![[Pasted image 20241001135603.png]]
- user permissions is the first three
	- r - read
	- w- write
	- x - executable 
- next 3 is group permissions
- last 3 is other permissions

### Linux Users
- users generally divided into two groups
	- **human users** - a person interacting with a computer
	- **system users** - users created to isolate resources
- users have a UID (user ID) and belong to at least one group
- when you create a new user, you also create a new group
	- the group has a GID (group ID)
- usually group ID and user ID are the same but they don't have to be
	- standard to just keep them the same
- `UID 0` is `root`
- UID 0-999 are for system users
- UID 65534 is user `nobody`
- UID 1000 - 65533 and 65535 to 4294967294 are regular users
- every user has a home directory
	- for the root user, it is in `/root`
	- other users have it in `/home/user-name`

#### Where is this information stored?
- stored in `etc/passwrd`
- `etc/passwrd` stores the following info:
	- username
	- password
		- an x next to it indicates that there is an encrypted password stored in the `/etc/shadow` file
	- UID
	- GID
	- name or GECOS
	- location of home directory
		- generally `/home/<user-name>`
	- location of login shell 
![[Pasted image 20241001141139.png]]

#### `/etc/shadow`
- shadow file stores users encrypted passwords as well as information about the users password
	- login name
	- encrypted password
	- Days since January 1, 1970, that password was last changed
	- Days before password may be changed
	- Days after which password must be changed
	- Days before password is to expire that user is warned
	- Days after password expires that account is disabled
	- Days since January 1, 1970, that account is disabled
	- A reserved field
![[Pasted image 20241001141528.png]]

#### managing accounts/group commands
- `adduser or useradd` - add a user to the system
- `userdel` - delete a user account and related files
- `addgroup` - add a group to the system
- `delgroup` - remove a group from the system
- `usermod` - modify a user account
- `chage` used to change the password expiration time and see user password expiry information
- `passwd` used to create or change a user account's password
- `sudo` run one or more commands as a superuser 

### Creating a new use with `useradd`
#### `useradd` vs `adduser`
- `useradd` command used to create a new user
	- this command is available on almost every Linux OS
- `adduser` does the same thing but isn't available on every Linux OS
- better to use `useradd` for all of our new users

#### `useradd`
```bash
useradd [options] <user-name>
```

- default configuration for `useradd` utility can be found in `/etc/default/useradd`
- copies files from `/etc/skel` into our new user's home directory 
##### Options
- `-m` create home directory
- `-d` specify a different home directory path for the user
- `-s` specify the login shell
- `-G` add the user to other groups as well

- Default configuration for the useradd utility can be found in `/etc/default/useradd`.
### delete users with `userdel
- delete users with the `useerdel` command
```bash
`userdel [OPTIONS] USERNAME`
```

- `-r` to remove a user and their home directory
- `-f` to forcefully delete a user

### switching users
- `su` to switch to the user account
```bash
su -l packt
```
- `su` alone switches to another user while maintaining the current environment
- `su -` simulates a complete login environment for the target user
- `-l` option ensures that the environment is set up as if the user had just logged in, rather than simply switching to the user’s account

### setting a password with `passwd`

```bash
`passwd <user-name>`
```

#### locking a user account
- `-l` to lock a user account
- `-u` to unlock
```bash
`sudo passwd -l $user-name`
```

```bash
`sudo passwd -u $user-name`
```

### groups
- a group in Linux is a collection of users
	- easier to assign permissions to multiple users
- `/etc/group` contains info about the groups that exists on your system
- can view members of a group with `groups` command
```bash
groups <user-name>
```

- create a new group with `groupadd`
```bash
groupadd [OPTIONS] <group-name>
```

### the `sudo` command
- `sudo` command allows a user to temporarily perform tasks with elevated privileges 
- `/etc/sudoers` has configuration for sudo
- sudo has some configuration in the `/etc` directory 
	- e.g. configuration for the sudo command, who can use sudo, a directory containing individual configuration files 
- `wheel` is a special user group that controls which users can gain root access.
- The **`sudo`** command and the **wheel group** are both related to managing user privileges and executing commands with elevated permissions

Like other utilities in most Linux OSs, `sudo` has some configuration in the `/etc` directory.

| File            | Purpose                                               |
| --------------- | ----------------------------------------------------- |
| /etc/sudo.conf  | Configuration for the sudo command                    |
| /etc/sudoers    | Configuration for who can use sudo                    |
| /etc/sudoers.d/ | A directory containing individual configuration files |

The sudoers file will generally include a line that grants members of either the "sudo" group or the "wheel" group sudo privileges.

For example, the sudoers file on my system contains the line `%wheel ALL+(ALL) ALL` Which grants members of the wheel group the ability to do everything with sudo.

### `usermod`
- `usermod` modified an existing users account attributes
```bash
usermod [options] <username>
```

#### options
- `-aG` add user to one or more groups without removing them from any existing group
- `-g` changes the user's primary group
- `-d` change home directory 
	- you can include `-m` to move their files to the new home directory too
- `-l` renames the user
- `-e` set account expiry date 

### add a user to the wheel group to allow them to use sudo in Arch

We have two options when it comes to granting a user sudo privileges. If you look in the `/etc/sudoers.d` directory on our Arch droplets there will be a `90-cloud-init-users` file. This was created by the cloud init YAML configuration added when you first created your server.

We could add another file here for a new user, but the more common way to grant sudo privileges to a user is to give sudo privileges to a group then make any user that we want to have sudo privileges a member of that group. Arch uses the "wheel" group by default so we are going to stick with this.

The first part of this is editing the `/etc/sudoers` file. We need to uncomment this line `# %wheel ALL=(ALL:ALL) ALL` Remove the octothorpe and spaces, so that the line starts with '%'.

By default the `visudo` command wants to use vi. But it will respect EDITOR settings. so you can use the command

```
sudo EDITOR=nvim visudo
```

You can also edit the file with vim and sudo. But it is recommend to use visudo

> [!warning] sudo nvim configuration When you run the command `sudo nvim $file` you will open vim as the root user. Since your neovim configuration is not in the root users home directory none of your configurations will be set. You can get around this by specifying a configuration file with the -u option. `sudo nvim -u /home/<user-name>/.confg/nvim/init.lua $file-to-edit` Because this is a longer command you may want to make this an alias in your .bashrc.

After uncommenting that line in the sudoers file members of the wheel group can use sudo. So the next step is to add our new regular user to the wheel group.

```
usermod -aG wheel <user-name>
```

> [!question] What do the a and G options do in the command above?

you can see which groups a user belongs to with the `groups` command.

```
sudo groups $user-name
```

### Permissions and ownership 
- linux OS uses permissions in individual resources (files) to determine who can do what with a file
	- r = read
	- w = write
	- x = execute
	- - = placeholder for a permission a user doesn't have
- these permissions are granted on a file to:
	- user
	- group
	- other
- permissions can be represented in two ways:
	- symbolic representation (r,w,x)
	- numeric or octantal (0-7)
		- r = 4
		- w = 2
		- x = 1
- ![[Pasted image 20241001144241.png]]

#### Changing file permissions with `chmod`
- use `chmod`to change file permissions
	- can be written numerically or symbolic 
```bash
- '+' add a permission
- '-' remove a permission
- '=' set a permission explicitly
```
- `chmod u+x file` will add execute permissions to user
- `chmod a+x file` adds execute permissions to the user, group, and other
- `chmod u+x, g+w, o-r file` adds execute to the user, write to the group, and removes read from the user 
- `chmod u=rw, g=r, o= file` sets the user to read and write, group to read, and other to nothing

#### changing file ownership with `chown`
- `chown` used to change either user or group owner of a file
```bash
chown [OPTIONS] USER:[GROUP] <file>
```

Examples:

```
chown user-name:group-name file-name
# change both user and group
```

```
chown :group-name file-name
# change only the group owner of a file
```

```
chown -R www-data: /var/www
# -R for recursive
```


#### changing group ownership with `chgrp`
- changes the group ownership of a file or directory
```bash
chgrp [options] <groupname> <file/directory>
```
- add `-R` to do it recursively 


## Lecture 6 - Bash and Scripts

### Script names and locations
- use `.sh` extension for windows users to make it easy to recognize the script as a shell script
- because of $PATH restrictions, scripts cannot be executed from the current directory
- consider using `~/bin` to store scripts for personal use
- `/usr/local/bin` for scripts available to all users 

### running scripts
- to run a script as a program, you need to set the execute permission
	- `chmod +x myscript`
- if the directory containing the script is not in $PATH, then you need to run it like 
	- `./myscript`
- scripts can also be started as an argument to the shell
	- `bash myscript`
		- bash is the executable and the script is the argument to bash
		- this wouldn't work if the script was meant for a different interpreter 
### Shebang
- the first line of a shell script
- tells the shell which interpreter to use when the script is executed 
- two ways to write a shebang:
	- `#!/bin/bash`
	- `#!/usr/bin/env bash`
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

### single vs double quotes
- double quotes expand variables
- single quotes are literal
	- it will literally do what is inside the single quotes

```bash
animal=cat
echo "$animal"
cat
#reads the value inside of animal by expanding it 


echol '$animal'
$animal #literally printing animal
```

### echo
- the built in `echo` display text or the value of variables to the terminal
	- it will print a `/n` new line after the echo line is executed
- echo can be used to print multiple lines and will preserve white space

### variables in bash
- variables can be declared like this: `animal="wolf"`
	- **make note that there is no space after the =**
- local variables only work in the current shell
-  to clear variable contents, you can use `key=`
	- this wipes the value of the variable without deleting the variable
- use `unset` if you want to delete the variable also
- use `env` to get access to environment variables 

#### using variables
- to use a variable in your code, prepend a `$`
	- `echo $animal`
- when calling a variable, it will look at these 3 places to find it
	- local shell variables (built ins)
	- environment variables ($PATH)
	- functions 
- recommended (but not mandatory) to put the variable name between { }
	- e.g. `echo ${color}`
	- if you want to do operations on the value of a variable while working on it, then curly brackets are mandatory

#### environment variables
- environment variables are accessible in sub shells
- `export variable="value"`
	- **this makes an environment variable that is accessible in sub scripts** 
- using upper case for environment variable names is popular convention
	- e.g. PATH or EDITOR

#### Special Variables
- bash works with some special variables, that have an automatically assigned value
	- `$RANDOM`: a random number
	- `$SECONDS`: the number of seconds this shell has been running
	- `$LINEO`: the line in the current script
	- `$HISTCMD`: the number of this command in history
	- `$GROUPS`: an array that holds the name of groups that the current user is a member of
	- `$DIRSTACK` list of recently visited directories, also use dirs to display

### the `declare` built in
- you can use `declare` to provide attributes to a variable 
- you can use `declare -p` to find out which type of variable something is 

```bash
# declare a read only variable
declare -r CONFIG=file

# declare a variable as an integer
declare -i NUM
```

### Positional Parameters 
- **positional parameters** are special variables that allow you to access arguments passed to a script or a function
- `$0` is the path to the script
- `$1` is the first argument
- `$2` is the second argument

#### key positional parameters
- `$$` process ID of the current shell
- `$#` the total number of arguments passed to the script
- `$@` the value of all arguments passed to the script
- `$?` exit status of the last executed command
- `$!` the process ID of the last executed command
- `$*` all arguments treated as a single string

### conditionals
- `test` built in can be used to evaluate expressions

#### if statements
```bash
if [[ test ]]; then
	consequent
fi
```

#### elif and else conditions
```bash
if [[ test ]]; then
	consequent
elif [[ second test ]]; then
	consequent
else
	consequent
fi
```

#### testing with `[[]]` vs `[]`
- `[[ condition ]]` has more features
	- provides more powerful and versatile testing capabilities, with improved handling of strings, pattern matching, and logical operations.
- `[ condition ]` conditionals will run in more shells
	- since we only use bash for the class, we use `[[ condition ]]`

### loops
- can use `break` to exit a loop immediately
- `continue` skips the rest of the current loop and proceeds to the next iteration 

#### for loop
```bash
for variable in list;
do
    # commands to be executed
done
```

```bash
#!/bin/bash

for i in {1..5}; do
    echo "Number $i"
done
```

#### until loop
```bash
until [[ condition ]];
do
    # commands to be executed
done
```

#### while loop
```bash
while [[ condition ]];
do
    # commands to be executed
done
```

### indexed and associative arrays
- bash arrays cannot be more than one dimensional so you can't nest an array inside an array
- indexed array = list
- associative arrays = dictionary 

#### indexed array
- creating an indexed array
	- use `declare -a`
```bash
declare -a arr=("1" "2" "3")
# or
arr=("1" "2" "3")
# Array items are separated by whitespace
```

- accessing indexed array values
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

#### associative array
- creating an associative array
	- use `declare -A`
```bash
declare -A person
person["name"]="Jan"
person["age"]="31"
```

- adding in new key value pairs
```bash
array_name[key]="value"
```

- accessing values and keys in an associative array
```bash
# print all values
echo "${ex_arr[@]}"
# print all keys
echo "${!ex_arr[@]}"
# print value by key
echo "${ex_arr["key1"]}"
```
- You can retrieve all keys or values from an associative array:
	- Keys: Use `${!array_name[@]}`
	- Values: Use `${array_name[@]}`

- deleting a key-value pair
```bash
unset array_name[key]
```

### command substitution
- you can store the output of another command in a variable using command substitution
- two common ways to do command substitution
	- using back ticks
	- using $(command) 
		- this is the preferred way

```bash
variable=`command`
```

```bash
variable=$(command)
```

- e.g. we can store the output of a date command in a variable like this:
```bash
now=$(date)
```

## Lecture 7 - bash functions and case statements

### shell expansions
- shell expansions allow the shell (like bash) to transform text, substitute variables, and interpret commands
	- expansions are performed on the command line after a command has been split into token(s)
- there are seven kinds of expansions performed
	1. brace expansion
	2. tilde expansion
	3. parameter and variable expansion
	4. command substitution
	5. arithmetic expansion
	6. word splitting
	7. filename expansion

#### brace expansion
- brace expansions allow you to generate a sequence of string by expanding a pattern within curly braces 
	- useful for creating sequences or generating multiple variations of a string

```bash
echo {1..5..2} # returns 1 3 5
echo {a..f..2} # returns a c e
echo file{1..3} # returns file1 file2 file3
echo file{1,2,3}.txt # returns file1.txt file2.txt file3.txt
```

#### tilde expansion
- `~` is the tilde expansion
	- it represents the home directory of the current user or other users
- `~` -> `/home/username`

#### parameter and variable expansion
- parameter expansion allows you to reference the values of variables
- using curly braces around the variable name can help prevent ambiguity and clarify where the variable name ends

```bash
first="Nathan"
last="McNinch"
echo "${first}_${last}" # returns Nathan_McNinch
```

- it can also be used to replace characters
```bash
letters="abc"
echo ${letters/abc/123} # returns 123
```

- it can perform string slicing
```bash
name="Nathan"
echo "${name:0:2}" # returns Na

echo "${name:2:2}" # returns th
```

#### command substitution
- command substitution allows the output of a command to replace the command itself 
```bash
now=$(date)
dir=$(realpath "$1") # use realpath to convert a relative path to an absolute path
```
- so now you could just call `$now` instead of `$date` and get the same result

- command substitution allows you to run a command and use its output as part of another command
```bash
echo "Current directory is $(pwd)" # prints Current directory is /home/user
```

#### arithmetic expansion
- allows the evaluation of an arithmetic expression and the substitution of the result

```bash
$(( expression ))
```

```bash
echo "$(( (3+4) * 5 ))" # returns 35
```

#### filename expansion
- bash scans for each word of the characters  "`*`", "`?`", "`[`". 
	- if one of these characters appears and is not quoted, then the word is regarded as a pattern, and replaced with an alphabetically sorted list of filenames matching the pattern
- `*` matches any sequences of character
- `?` matches any single character
- `[]` matches any enclosed characters

```bash
ls d* # will display the directories starting with d

ls dir? # will display dir1 and dir2

ls *[1-2] # will display all of the directories that end with 1 or 2
```

### functions in bash
- a shell function is a compound command that is given a name
	- stores a series of commands for later execution 
- use `set` to see a list of all functions currently available
- use `unset` to remove a function 

you could write functions like this (we don't do it anymore)
```bash
function func_name() {
  function body
}
```

**we mainly write functions like this**
```bash
func_name() {
  function body
}
```
- functions in bash must be declared before they are called
- arguments are passed into functions using positional parameters

```bash
# declare a function
say_hi() {
  echo "hi $1"
}
# call a function
say_hi "Bob" # returns hi Bob
```

#### local variables
- by default all variables in bash are global
	- even variables in functions
- **variables can be defined as local to function scope using the `local` keyword** 

```bash
#!/usr/bin/env bash
localretrn () {
  local func_result="some result"
  echo "$func_result"
}

func_result="$(localretrn)"
echo $func_result
```

#### return a value from a bash function
- bash does include a `return` keyword but it doesn't return a value from a function
	- `return` is used to exit a function and return an exit status
- to return a value from a result, you would need to echo out the result from a function and save it in a a variable
```bash
function add {
    echo $(( $1 + $2 )) # print the value with echo
}

result=$(add 3 5) # capture the value printed in a new variable
echo "The result is: $result"
```

### `case` statements in bash
- similar to if/then/else clauses
- `case` has powerful pattern matching
- `case` statements are used for conditional branching based on pattern marking
- a `case` statement allows you to execute different blocks of code depending on the value of a variable or expression 

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
- `)` indicates that is a pattern we're trying to match
- `;;` indicates the end of the condition
- `*` matches any string of characters
	- good for default case for errors
- `?` matches any single character 
- you can use `[a-z]` to make a range in a case statement
- `|` separates alternate choices that match a particular branch
- cases need to end in `esac` to end the case 

### use `getopts` to write a script with options
- `getopts` is a shell built in the can help when handling options in shell scripts
- `getopts` is a function that takes 3 options
	1. valid options to be handled
		- e.g. `ab` in the below example
		- having `:` means that it requires an argument
		- a `:` at the beginning tells `getopts` to run in silent error checking mode or let the script define errors
	2. a variable populated with the options arguments 
		- e.g. `opt` in the example below
	3. a list of options and arguments to be processed `$@`

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
- when a flag is set to expect an argument, the arguments for that flag are held in `OPTARG` variable
	- e.g. `my_script -n "ted"` will have "ted" saved in the `OPTARG` for `-n`
- `OPTARG` is a special variable used with `getopts` to store the arguments that follows an option when an option requires one
- the string `abf:` tells `getopts` that `-a` and `-b` are options that need no arguments but `-f` needs an argument because of the `:`
	- that `-f` argument will get stored in `OPTARG`

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

### `shift` positional parameters 
- `shift` moves positional parameters *n* characters to the left 
	- default is 1 character 

```bash
#!/bin/bash
echo $1 $2 $3 # prints 1 2 3
shift
echo $1 $2 $3 # prints 2 3 4
shift 2
echo $1 $2 $3 # prints 4, it shifts 2 additional positions
```

![[Pasted image 20241014152000.png]]
### OPTIND
- when dealing with a mix of `getopts` and positional arguments, **the flags and flag options should always be provided before the positional arguments** 
	- we want to parse and handle all flags and flag arguments before positional arguments
- `OPTIND` is a special variable used with `getopts` to keep track of the index of the next argument to be processed 

e.g. shifting the positional parameters to remove an options already handled by `getopts`
```bash
`shift $(($OPTIND -1))`
```
- it also works if your options accept arguments

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

### here documents
- a <mark style="background: #ABF7F7A6;">here document </mark> is used an Input/Output redirection to feed a command list to an interactive program of command
- used as a scripted alternative that can be provided by input redirection
- useful as all code that needs to be processed is part of the script and no need for external commands 
- The heredoc makes it easy to handle long strings of text in shell scripts or command-line input

![[Pasted image 20241014152218.png]]
- `ENDSESSION` is a keyword that marks the identifier for the here document
- `<<` is the input direction