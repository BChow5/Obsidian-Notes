### Git Review
-  Git is a distributed version control system 
- git has 3 important features
	- keeping track of files in versions (snapshots in time)
	- branches, so you can work on new features and bug fixes independently
	- you can work on projects with other people

#### Git configuration

Before we start using Git we need to do a minimal amount of configuration. Run the command:

```
git config --list
```

If you see something that looks like this:

```
user.email=user-name@email.com
user.name=some-user-name
```

You can skip the rest of the configuration. If not run the following commands

```
git config --global user.name your-name
git config --global user.email your-email-address
```

This will create a `~/.gitconfig` that stores this information.

#### [](#initialising-a-local-repository-or-cloning-a-remote)initialising a local repository or cloning a remote

We could start using Git by adding Git to an existing project with the command:

```
git init
```

Or you could create a new remote repository on GitHub/GitLab first and clone that to your local machine with the command

```
git clone git@your-remote-repo
```

#### [](#checking-a-local-repositories-status)Checking a local repositories status

```
git status
```

#### [](#add-a-file)add a file

```
git add file-name
```

#### [](#commit-a-change)commit a change

```
git commit -m 'add new git content to class'
```

#### [](#push-changes)push changes

```
git push
```

#### [](#alternatives-to-git)alternatives to Git

This is just included for anyone curious about Git alternatives.

- [mercurial](https://www.mercurial-scm.org/)
- [darcs](https://darcs.net/)
- [pijul](https://pijul.org/)

#### [](#learning-git-resource)learning Git resource

[Git for Complete Beginners: A Visual Learning Journey for the Basics of Git](https://learning.oreilly.com/course/git-for-complete/0642572059965/) This is a short video series that you can go through to learn more about using Git.

### The Linux file system
- A filesystem is the method and data structures that an operating system uses to keep track of files on a disk or partition
	- the way the files are organized on the disk
- we can think of it as underlying rules for how data is stored
	- common Linux file systems are things like BTRFS, EXT4, etc.
- a filesystem is also the higher level organization of files on a system

#### Filesystem Hierarchy Standard
![[Pasted image 20240915110507.png]]
- most Linux distros have files and directories organized according to the <mark style="background: #ABF7F7A6;">Filesystem Hierarchy Standard </mark>
- the standard allows for users and developers to know where important files and directories are found on a system
- the filesystem begins at the root directory `/` and branches out like a family tree
- you can find out more using the man page `man hier`
##### root `/`

The root directory, `/` is the start of our tree, or the beginning of our directory structure.

Inside of the root directory are key directories used to store specific files on your system.

##### [](#boot-boot)boot `/boot`

`boot` contains static files for the boot loader.

`boot` stores data that is used before the kernel begins executing user-mode programs.

##### [](#dev-dev)dev `/dev`

`dev` contains special, or device files

##### [](#etc-etc)etc `/etc`

The `etc` directory contains configuration files
* passwords are stored in "shadow" not "password"
	* this is because the password directory was unsecure 

##### [](#bin-bin---usrbin)bin `/bin` -> `/usr/bin`

The `/bin` directory historically stored utilities needed to run in single user mode. Now bin is generally a symbolic link that points to /usr/bin

##### [](#proc-proc)proc `/proc`

The directory /proc contains (among other things) one subdirectory for each process running on the system, which is named after the process ID (PID).

proc is a pseudo filesystem

**reference:**[https://www.kernel.org/doc/html/latest/filesystems/proc.html](https://www.kernel.org/doc/html/latest/filesystems/proc.html)

##### [](#var-var)var `/var`

contains variable data files. `/var/log` for example

##### [](#home-home)home `/home`

Regular users home directories are stored here. For example, if you have a used named kim, kim's home directory would be in `/home/kim`

```ad-question
Mid term question:
Where is your home directory? 

your home directory is inside your user folder that is inside the home folder that is inside the root directory
```
#midterm 

##### Pseudo Filesystems
- a pseudo filesystem is a way to work with non-file data as through it were a file
- the advantage to this method is that organizing and interacting with this data can be handled in the same, familiar way that you work with files 

### Symbolic Links
- a hard link is a pointer to a file (the directory entry points to the inode)
- symbolic link is an indirect pointer to a file (the directory entry contains the pathname of the pointed-to file. a pointer to the hard link to the file )
- advantage
	- you cannot create a hard link to a directory, but you can create a symbolic link to a directory
- dereferencing symbolic links
	- when you dereference a symbolic link you follow the link to the target file
- creating a symbolic link
	- you can create a symbolic link with the `ln` utility using the `-s` options 

```
ln -s [OPTIONS] FILE LINK 
ln -s source_file symbolic_link
```

#### Viewing links with `ls`
* When you use the the `ls` utility with the `-l` option links will appear with an arrow pointing to the file they link to. In the example below 'bin' is a link that points to 'usr/bin'

### Cloud-init
- cloud-init comes setup on many Linux distros, particularly distros intended to be run on a server. 
	- This is why we had to get the the cloud image when we downloaded the Arch Linux image we are using.
- Additionally most cloud service providers offer several ways of passing cloud-init configuration to a server.
- We can confirm that cloud init is running by running the command
- cloud-init allows us to setup a server with some initial configuration
#### [](#troubleshooting-a-cloud-config-file)troubleshooting a cloud config file

If the configuration in your file is not being applied you probably won't see an error message. You can find one in the logs (`journalctl -b`).

The first place you should look is your YAML file. YAML is picky about white space, you may need to change some settings in your text editor, particularly if you are running Windows as your host machine.

Reference:

- [Arch Wiki Cloud-init](https://wiki.archlinux.org/title/Cloud-init)