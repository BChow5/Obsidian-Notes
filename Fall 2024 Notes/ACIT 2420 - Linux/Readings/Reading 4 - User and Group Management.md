### Overview of managing accounts/groups
- linux allows multiple users to be logged in and work simultaneously on a single machine
- not a good idea to let users share the same login info the same account
- access to specific system resources like directories or files may need to be shared by two or more users
	- can achieve this using user and group administration features
- **general/normal users** and **root/superusers** are the two categories of users in Linux systems 
- custom rights of user and group accounts are maintained by each user logging in to the operating system using a different set of credentials
- adding new users requires specific permissions (superuser)
	- also true for other admin operations like account deletion, account update, group addition and deletion 

##### These operations are performed using the following commands:
- **adduser** - add a user to the system
- **userdel** - delete a user account and related files
- **addgroup** - add a group to the system
- **delgroup** - remove a group from the system
- **usermod** - modify a user account
- **chage** - used to change the password expiration time and see user password expiry information
- **passwd** - used to create or change a user account's password 
- **sudo** - run one or more commands as another user (typically with superuser permissions)


##### Users
- Files relevant to these operations include **/etc/passwd** (user information), **/etc/shadow** (encrypted passwords), **/etc/group** (group information), and **/etc/sudoers** (configuration for **sudo**).
* Superuser access is granted by using either the **su** command to become the root user or the **sudo su** command to get root privileges.
* These are the default locations for user account information:
	* User account properties: **/etc/passwd**
	* User password properties: **/etc/shadow**
* A group with the same username is also created when a user is created. 
* Every user has a home directory; 
	* for the root user, it is placed in **/root**; for all other users, it is in **/home/**.
	* #midterm 

##### `/etc/passwd` file
- contains all of the account details
```
<username>:<x>:<UID>:<GID>:<Comment>:<Home directory>:<Default shell>
```
![[Pasted image 20240930201956.png]]

### How to add a new account
- two commands that can be used to generate new users:
	- <mark style="background: #ABF7F7A6;">adduser</mark>
	- <mark style="background: #ABF7F7A6;">useradd</mark>
- they achieve the same thing but in different ways

#### Using useradd
- you need sudo capabilities to add an account if you don't have root access
	- this must be defined in `/etc/sudoers`

##### example
```bash
sudo  useradd -d /home/packt -m packt
```
- setting up a new user with the name packt with this command
- confirming that we want a home directory to be established for this user by using the `-d` option 
	- then we specified `/home/packt` as the user's home directory 
- if we hadn't used `m` parameter, the system would not have known that i wanted my home directory to be created while the process was running 

The following command can be used to set a password for the newly created packt account
```bash 
~ $sudo passwd packt
Changing password for user packt.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

#### Using adduser

##### Example
```bash
~ $sudo adduser packt2
Adding user `packt2' ...
Adding new group `packt2' (1004) ...
Adding new user `packt2' (1003) with group `packt2' ...
The home directory `/home/packt2' already exists.  Not copying from `/etc/skel'.
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for packt2
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n]
```
- The command copied files from **/etc/skel** into our new user’s home directory 
- it set the user’s home directory to **/home/packt2** by default.
- The user account was also assigned the next available **User ID** (**UID**) and **Group ID** (**GID**) of **1004**.
- In point of fact, the **adduser** and **useradd** commands both copy files from the **/etc/skel** directory;
- however, the **adduser** command is somewhat more detailed in the tasks that it executes.

### How to Delete an Account
- We will make use of the <mark style="background: #ABF7F7A6;">userdel</mark> command in order to delete a user account
- important to note if you will need to access the content of this old user
- The user’s home directory’s contents are not deleted when the **userdel** command is used since this behavior is not the default.

we will delete **packt2** from the system by executing the following command:

```bash
sudo userdel packt2
```

If we want to remove the home folder once with the account, we need to use the `–r` parameter:

```bash
sudo userdel –r packt2
```

- Before deleting users’ accounts, remember to check whether the files in their home folders are needed. 
	- Once deleted, they cannot be recovered if there is no backup
- deleting a user account in Linux involves backing up data, terminating processes, removing the user from groups, deleting the home directory, updating system files, and performing a final cleanup.
	- By following these steps, you can securely delete an account while managing the associated files and permissions effectively.

### Understanding the/etc/sudoers file
- let’s see how to use the ordinary user account we created earlier to carry out user administration operations.
- We must make a special permissions entry for **packt** in **/etc/sudoers** in order to allow it special access:
```bash
packt ALL=(ALL) ALL
```
- First, we state to which user this rule applies (**packt**).
- All hosts that use the same **/etc/sudoers** file are covered by the rule if the first **ALL** is present. Since the same file is no longer shared among different machines, this term now refers to the current host.
- Next, **(ALL) ALL** informs us that any user may execute any command as the **packt** user. In terms of functionality, this is similar to **(root) ALL**

### Switching users
- We are now prepared to begin using the **packt** account to carry out user administration duties
- Use the **su** command to switch to that account to accomplish this.

```bash
su -l packt
```
- you're able to check permissions for our newly formed pact account by using the sudo command

```bash
~ $sudo adduser packtdemo
Adding user `packtdemo' ...
Adding new group `packtdemo' (1005) ...
Adding new user `packtdemo' (1004) with group `packtdemo' ...
The home directory `/home/packtdemo' already exists.  Not copying from `/etc/skel'.
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for packtdemo
Enter the new value, or press ENTER for the default
Full Name []:
Room Number []:
Work Phone []:
Home Phone []:
Other []:
Is the information correct? [Y/n]
```
- makes a new account called packtdemo
- Changes to the user’s home folder, default shell, and the ability to add a description to the user account can all be made with the **usermod** command.
- Changing to an alternate user account is frequently quite beneficial when working with support (especially while troubleshooting permissions)
- You can switch to root user by running the **sudo su** or **su –** command, or just simply **su**.
- **su** alone switches to another user while maintaining the current environment, while **su –** simulates a complete login environment for the target user, including setting up their home directory and environment variables and starting a new login shell.
	- The choice between the two commands depends on the specific requirements or tasks you need to accomplish as the user switched to.

### Managing account passwords
- the **passwd** command enables us to alter the password for the user who is now logged in to the system. 
- In addition, we are able to change the password for any user account on our system by running the **passwd** command while logged in as root and providing the username.

#### locking/unlocking user accounts
- <mark style="background: #ABF7F7A6;">passwd</mark> can also lock and unlock a user account 

Use the **-l** option when you want to lock an account. For example, to lock the account for the **packt** user, we use the following command:
```bash
sudo passwd -l packt
```

Unlock the account
```bash
sudo passwd –u packt
```

#### Setting password expiration 
- We may use <mark style="background: #ABF7F7A6;">chage</mark> to change the length of time for which a user’s password is valid
	- it also provides a more user-friendly alternative to reading the **/etc/shadow** file in order to view the current password expiration information

By giving a username and using the **-l** option of the **chage** command, we are able to view the pertinent information:
```bash
sudo chage -l packt
```
- It is not necessary to run **chage** as **root** or with the **sudo** command
	- no need to raise your permission level to be able to view the expiration information for your own login
- To access information using **chage** for any user account other than your own, however, you will need to utilize **sudo**
![[Pasted image 20240930213052.png]]
- Using the **chage** command, you can require a user to change their password upon their first successful login to the system
	- This is done by changing their total number of days before password expiration to **0** in the following manner

```bash
sudo chage -M 90 <username>
```
- configuring the user account to become invalid after 90 days and to demand a new password at that time

### Group management
- You only need to use the **cat** command to read the contents of the **/etc/group** file

```bash
sudo usermod –aG packtgroup packt
```

The **-aG** option is used to add a user to a specific group. The **-a** flag means _append_, which means that the user will be added to the group without removing them from any other groups they may already be a member of. The **G** flag specifies the group name.

### Permissions
- Change file permissions with **chmod**
- Change the file owner with **chown**
- Change group ownership with **chgrp**
- Print the user and group IDs with **id**

We’ll use **chmod** to modify a file’s permissions. A symbolic representation indicating to whom the new permissions will applied must come after this command:

- **u** means user
- **g** means group
- **o** means all other users
- **a** means all users

The types of permission are as follows:

- **+r** adds read permission
- **-r** removes read permission
- **+w** adds write permission
- **-w** removes write permission
- **+x** adds execute permission
- **-x** removes execute permission
- **+rw** adds read and write permissions
- **+rwx** adds read, write, and execute permissions

All of these permissions can be expressed numerically as follows:

Read – **4** : Write – **2** : Execute – **1**

### Changing groups
The **chgrp** command will be discussed now in the context of making **packtgroup** the new owner of **testfile**. After the command, we specify the name of the group and the name of the file whose ownership is to be changed (in this case, **testfile**):

sudo chgrp packtgroup testfile

Let’s check the ability of user **packtdemo** to write to this file now. A permission refused error should appear for the user. We can set the relevant permissions for the group to allow **packtdemo** to write to the file:

sudo chmod g+w testfile

Then use **usermod** once more to add the account to **packtgroup**, this time using the **-aG** combined option as follows:

sudo usermod -aG packtgroup packtdemo

The abbreviation for _append to group_ is **-aG**.

We can use **chown** followed by the usernames and filenames, in that order, to make **packtdemo** the owner of **testfile** rather than just adding the user to the **packtgroup** group:

sudo chown packtdemo testfile