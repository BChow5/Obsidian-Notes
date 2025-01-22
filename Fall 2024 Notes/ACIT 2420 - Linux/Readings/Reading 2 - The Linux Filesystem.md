### What is a filesystem?
-  a filesystem governs the storage and retrieval of data in computers
- each collection of data is referred to as a file
- a file system is the name for the organization framework and logical principles used to handle names and groups of bits of data

##### What type of Linux file systems are there?
- **ext**
	- first filesystem constructed expressly for linux
	- maximum file size grew to 2 GB as a result of the new metadata structure that was created. The maximum length of filenames was also increased at the same time to 255 bytes.
- **ext2**
	- introduced innovations in fields such as storage capacity and overall performance. The maximum file size was notably increased to 2 TB, as opposed to the previous version’s 2 GB. Still, filenames remained limited to 255 bytes long.
- **ext3**
	- The 2-TB maximum file size did not change, but ext3 was superior to ext2 in that it is a journaling filesystem
	- ext3 supports three levels of journaling:
	- **Journal**: In the event of a power outage, the filesystem ensures effective filesystem recovery by writing both user data and metadata to the journal. Of the three ext3 journaling modes, this is the slowest. This journaling mode reduces the likelihood that any changes to any file in an ext3 filesystem will be lost.
	- **Writeback**: When using the **data=writeback** mode, only metadata updates are logged in the journal. Data updates, on the other hand, are written directly to their respective locations on the disk without being logged in the journal first. This approach can provide better performance for write-intensive workloads because it reduces the overhead of journaling data.
	- **Ordered**: This mode does not update related filesystem metadata; instead, it flushes changes from file data to disk before updating the associated filesystem metadata. This is ext3’s default journaling mode. Only the files that were in the process of being written to the disk _disappear_ in the event of a power outage. The architecture of the filesystem is undamaged.
- **ext4**
	- This filesystem is commonly used as the default filesystem for the majority of Linux distributions since it overcomes a number of shortcomings that the third extended system had
	- can handle filesystems up to 1 exabyte (1 EB) and individual files up to 16 terabytes (16 TB). Additionally, a directory in ext4 can have up to 64,000 subdirectories (as opposed to 32,000 in ext3)
	- - **JFS**: JFS stands for _Journaled File System_. It is a 64-bit filesystem developed by IBM. In 1990, the first version of JFS (also known as _JFS1_) was introduced for use with IBM’s AIX operating system.
- **XFS**
	- XFS was designed as a high-performance 64-bit journaling filesystem. Large file manipulation and high-end hardware performance are strengths of XFS. In SUSE Linux Enterprise Server, XFS is the default filesystem for data partitions.
- **Btrfs**
	- Btrfs is a logging-style filesystem that links the change after writing the block modifications in a new area as opposed to journaling them. New changes are not committed until the last write.
- **Swap**
	- When the amount of memory available to the computer begins to run low, the system will use a file known as a swap file to generate temporary storage space on a solid-state drive or hard disk. The file replaces a section of memory in the RAM storage of a paused program with a new part, making memory available for use by other processes.

#### High Scalability
-  by leveraging allocation groups, XFS provides excellent scalability
	- the block device that supports the XFS file system is split into eight or more linear regions that are all the same size at the moment the filesystem is created
		- referred to as **allocation groups**
	- each allocation group controls its own disk space and inodes
	- the kernel can address multiple allocation groups at once
	- this feature is what allows XFS to have high scalability 

#### High performance
- XFS provides high performance by effectively managing disk space 
- XFS uses B+ trees to manage free space and inodes, improving its scalability and performance

### What filesystem does my system use?
- using the **df -T** command
- ![[Pasted image 20240914194053.png]]

### FUSE filesystem
- it's common practice for processes to make use of system calls to the kernel in order to read or write to a mounted filesystem
	- however, you do have access to fata from the filesystem that doesn't seem to belong in the users domain
	- the stat() system call in particular returns inode numbers and link counts
	- This information is made available to user-mode programs for the primary purpose of maintaining backward compatibility
- On non-traditional filesystems, it’s possible that you won’t be able to carry out operations that are typical of the Unix filesystem
	- e.g. you cannot use the **ln** command to create a hard link on a mounted VFAT filesystem because that filesystem’s directory entry structure does not support the concept of hard links

### The Directory Tree and Standard Directories
-  command: `tree -L 1`
	- shows the main structure of the root folder 

##### Linux filesystem commands
- **/bin**: the majority of your binary files are kept in this location
	- often used by Linux terminal commands and essential utilities such as **cd**(change directory) **pwd**(print working directory), **mv**(move)
- **/boot**: all the boot files afor the linux are found in this folder
- **/dev** your physical devices, such as hard drives, USB drives, and optical media are mounted here
	- your drive may have different partitions like /dev/sda1,/dev/sda2
- **/etc**: this directory stores configuration files
	- users can keep configuration files in their own /home folder but that would affect only the given user
	- using /etc for configurations will affect all users on the system
- **/home**: because this directory contains all your personal information, you'll spend most of your time here
	- the /home/username directory contains the desktop, documents, downloads, photos, and videos directories
- **/lib**: all the library buildings
	- always extra libraries that start with lib-something that get downloaded when you install a Linux distribution
- **/media**: this is where external devices such as USB drives and CD ROMS are mounted
	- varies between Linux distributions 
- **/mnt**: this directory basically serves as a mounting point for other folders or drivers
	- can be used for anything, although it is typically used for network locations
- **/opt**: this directory contains supplementary software for your computer that is not already managed by the package management tool that comes with your distribution 
- **/proc**: the process folder contains a variety of files holding system data
	- it gives Linux kernel. a mechanism to communicate the numerous processes that are active within the linux environment 
- **/root**: this is the equivalent of the /home folder for the root user, commonly known as the super user
	- you should only touch anything in this directory if you are really sure you know what you're doing 
- **/sbin**: this is comparable to the /bin directory, with the exception that it contains instructions that can only be executed by the root user
- **/tmp**: temporary files are kept here and are often erased when the computer shuts down, so you don't have to manually remove them as you would in windows 
- **/usr**: this directory contains files and utilities that are shared between users
- **/var**: the files used by the system to store information as it runs are often located in the /var subfolder of the root directory in Linux and other unix-like operating systems 
![[Pasted image 20240915110507.png]]

### Links (hard and symbolic)
- there are two alternative ways to refer to a file on a hard drive: hard links and symbolic links 
- the file system includes several approaches such as symbolic link and hard link 
- <mark style="background: #ABF7F7A6;">Hard Link</mark> basically refers to the inode of a file and is a synchronized carbon copy of that file 
- <mark style="background: #ABF7F7A6;">Symbolic Links</mark> point directly to the file, which in turn points to the inode, a shortcut 

#### What is an inode
- a unix-style filesystem uses a data structure called an <mark style="background: #ABF7F7A6;">indode</mark> to describe filesystem objects such as files and directories 
- the properties and disk block locations of an object's data are stored in each inode
- attributes of filesystem objects can include metadata, owner information, and permission information 
- inodes are essentially a whole address's numerical equivalent
- the operating system can obtain details about a file, including permission privileges and the precise location of the data on the hard drive, using an inode 

#### What is a Hard Link?
-  a hard link is a special kind of link that points directly to a specific file by its name
- A hard link will continue to point to the original file even if the file’s name is changed, unlike a soft link
- When comparing the two methods of linking a directory entry or file to the same memory region, hard links are more reliable
- As opposed to symbolic links, hard links prevent files from being deleted or moved

#### What are Symbolic Links?
- essentially shortcuts that refer to a file rather than the inode value of the file they point to
- this method can be applied to directories and can be used to make references to data that is located on a variety of hard discs and volumes
- a symbolic link will be broken or leave a dangling link if the original file is moved to a different folder
	- this is due to the fact that symbolic link refers to the original file and not the inode value of the file 
- because a symbolic link points to the original file, any changes you make to the symbolic link should result in corresponding changes being made to the actual file 

### Mounting and unmounting filesystems
- in order for the computer to access files, the file system must be mounted
- the `mount` command will show you what is mounted (usable) on your system at the moment 
- ![[Pasted image 20240915184619.png]]

#### How to unmount the filesystem
- using the `unmount` command and specifying the mount point or device, you can unmount (detach) the filesystem from your computer 
- ![[Pasted image 20240915184720.png]]

### Pseudo-filesystems
- a process information pseudo-filesystem is another name for the `proc` filesystem 
- it contains runtime system information rather than actual rules
- it can therefore be viewed as the kernel's command and information hub
- this directory (`/proc`) is accessed by many system utilities
- the `lsmod` command lists the modules loaded by the kernel, and the `lspci` command displays the devices attached to the PCI bus
- common examples of pseudo filesystems in unix-like operating systems include the following:
	- processes, the most prominent use
	- kernel information and parameters
	- system metrics, such as CPU usage 

### Processes
- all the information about each running process can be found in the `/proc/pid` file
- a synthetic filesystem is a filesystem that provides a tree-like interface to non-file objects, making them look like regular files in a disk-based or long-term storage filesystem
	- this type of filesystem is also known as a faux filesystem

### Kernel and System Information
Numerous folders under **/proc** contain a wealth of knowledge about the kernel and the operating system. There are too many of them to include here, but we will cover a few along with a brief description of what they contain:

- **/proc/cpuinfo**: Information about the CPU
- **/proc/meminfo**: Information about the physical memory
- **/proc/vmstats**: Information about the virtual memory
- **/proc/mounts**: Information about the mounts
- **/proc/filesystems**: Information about filesystems that have been compiled into the kernel and whose kernel modules are currently loaded
- **/proc/uptime**: This shows the current system uptime
- **/proc/cmdline**: The kernel command line