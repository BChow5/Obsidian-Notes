- a <mark style="background: #ABF7F7A6;">container</mark> is a lightweight, isolated environment that allows you to run applications and their dependencies independently of the host system. 
	 - Containers package everything the software needs to run, including the code, runtime, system tools, libraries, and settings
	 - less resource intensive that a virtual machine

### Users Groups and Permissions
- You have seen the output of the `ls -l` command a few times at this point
	- The output of this command nicely illustrates the relationship between users, groups and permissions
- Every file (remember that everything is a file) belongs to a user and a group (just one of each)
- files also have permissions for:
	- the user that owns the file
	- the group that owns the file
	- other (everyone else)
- those permissions are
	- read
	- write
	- execute
- with these things (users, groups, and permissions) we can determine who can do what 
![[Pasted image 20241001135603.png]]
- user permission is the first 3
- next 3 is for group permission
- last is other permission
- every file belongs to one user and one group
- a user can belong to multiple groups 

### Linux Users
- in general, we divide users into two groups:
	- <mark style="background: #ABF7F7A6;">human users</mark>, a person interacting with a computer
	- <mark style="background: #ABF7F7A6;">system users</mark>, users created to isolate resources 
- users have a UID (user id) and belong to at least one group
- when you create a new user, you also create a new group, which has a GID (group id)
- generally group id and user id are the same but they don't have to be
	- unless you have a good reason, just keep them the same
- UID 0 is `root`
- UID 1 to 999 are reserved for system users
- UID 65534 is user `nobody`
	- e.g. used for mapping remote users to some well-known ID, as is the case with "network file system"
	- https://learning.oreilly.com/library/view/learning-modern-linux/9781098108939/ch07.html#nfs
- UID 1000 to 65533 and 65535 to 4294967294 are regular users

### Where is this information stored?
- it is stored in `etc/passwrd`
- the `passwd` file in `etc` stores the following information:
	- username
	- password, x indicates that an excrypted password is stored in the /etc/shadow file
	- UID 
	- GID
	- name or GECOS
		- this field contains the following information, separated by commas:
			- user's full name or the application name
			- room number
			- work phone number
			- home phone number
			- other contact information
			- this is rarely used today
	- location of home directory, this is generally `/home/<user-name>`
	- location of login shell
![[Pasted image 20241001141139.png]]

- it is possible to read the `/etc/passwd` file as a regular user
```bash
cat /etc/passwd
```
- because the passwd file can be read by regular users and because it isn't good practice to store a password directly in plain text, passwords are stored in `/etc/shadow`

##### /etc/shadow
- the shadow file stores users encrypted passwords as well as information about the users password
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

### Creating a new user with useradd

#### useradd vs adduser
- the `useradd` command is a low level command that can be used to create a new user
	- this command is available on almost every Linux OS
- the `adduser` command isn't available on every Linux OS
	- on some (most ubuntu based distros) it is a perl script
	- on some, it is an alias to useradd, you you use useradd anyways
	- on many, it just isn't there
- so we are going to use `useradd` for all of our new users

#### useradd
- general syntax for the useradd command is:
```bash
`useradd [options] <user-name>`
```

to learn more about utilities options, see `man useradd`

##### Example command
Assume I want to make a new user "mo", the basic command would be 

```bash
`useradd mo`
```

#### Useradd configuration
- default configuration for the useradd utility can be found in `/etc/default/useradd`
- we can edit these defaults later 

### delete users with `userdel`
- you can delete a user with the `userdel` command
```bash
`userdel [OPTIONS] USERNAME`
```

- To remove a user and their home directory
```bash
`userdel -r $user-name`
```

- deleting a user can fail if a user has running processes. remember that you can kill a users processes with 
```bash
`pkill -u $user-name`
```

- or you can use the `userdel` command with the `-f` option
```bash
`userdel -fr $user-name`
```

### give the user a password with `passwd`
- our new user should also have a password
```bash
`passwd <user-name>`
```

- this will prompt you to enter a password twice. for security reasons, nothing will be displayed on your screen when typing the password

#### locking a user account
- the passwd utility can also be used to lock a user account
```bash
`sudo passwd -l $user-name`
```

- to unlock the account
```bash
`sudo passwd -u $user-name`
```

### groups
- a group in a Linux OS is a collection of users. groups can make it easier to assign permissions to multiple users
- you can view members of a group with the `groups` command
```bash
`groups <user-name>`
```

- you can create a new group with the `groupadd` command
```bash
`groupadd [OPTIONS] <group-name>`
```

- similar to `/etc/passwd`, the `/etc/group` file contains info about the groups that exist on your system

### the `sudo` command
- the sudo command allows a user to temporarily perform tasks with elevated privileges
- most linux systems include either a `sudo` command or a `doas` command, which also allows a user to temporarily run commands with elevated privileges
	- Arch includes the sudo command. `man sudo` to learn more.

#### The sudo and or wheel group
- `sudo` has some configuration in the `/etc` directory

| file            | purpose                                               |
| --------------- | ----------------------------------------------------- |
| /etc/sudo.conf  | configuration for the sudo command                    |
| /etc/sudoers    | configuration for who can use sudo                    |
| /etc/sudoers.d/ | a directory containing individual configuration files |
- the sudoers file will generally include a line that grants members of either the "sudo" group or the "wheel" group sudo privileges 
- For example, the sudoers file on my system contains the line `%wheel ALL+(ALL) ALL` Which grants members of the wheel group the ability to do everything with sudo.

#### add a user to the wheel group to allow them to use sudo in Arch 

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
- need it to be -aG
	- **adding user to a group**
- if you do just -G then you're overwriting your groups

> [!question] What do the a and G options do in the command above?

you can see which groups a user belongs to with the `groups` command.

```
sudo groups $user-name
```

### Permissions and ownership
- Linux OS uses permissions on an individual resource (file) to determine who can do what with a file
	- r = read
	- w = write
	- x = executre
	- - = a place holder for permissions the user doesn't have
- These permissions are granted on a file to either the:
	- user
	- group
	- other (everyone else)
- Permissions have two ways of being represented:
	- symbolic representation (rwx)
	- numeric (or octal) representation (0-7)
![[Pasted image 20241001144241.png]]

#### changing file permissions with `chmod`
- To change file permissions you can use the <mark style="background: #ABF7F7A6;">chmod</mark> utility
- chmod commands can be written either use symbolic or numeric methods

The symbolic method uses a combination of values that represent permissions and scope and operations that represent the change you want to make.

```
- '+' add a permission
- '-' remove a permission
- '=' set a permission explicitly
```

`chmod u+x file` will add execute permissions to the user.

`chmod a+x file` will add execute permissions to the user, group and other

`chmod u+x,g+w,o-r file` will add execute to the user, write to the group and remove read from other.

`chmod u=rw,g=r,o= file` will set the users to read and write, groups to read and other to nothing.

### Changing file ownership with `chown`
- <mark style="background: #ABF7F7A6;">chown</mark> command can be used to change the user or group owner of a file 

```
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
