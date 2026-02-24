### Understanding Active Directory: logical components
- **Organizational units (OUs)**
	- containers in a domain that allow you to organize and group resources for easier administration
		- including providing and delegating administrative rights 
	- can be used to group objects
		- e.g. printers, computers 
	- for policy
		- computer settings 
		- different departments may require different settings 
	- can drag people to new OUs and the settings will change
- **groups** are role based and for assigning permissions
	- IGDLA 

### Install from Media (IFM)
- **install from media** is an option that enables administrators to streamline the process of deploying replica domain controllers to remote sites
- using a cli tool called **Ntdsutil.exe**, admins can create domain controller installation media that includes a copy of the AD DS database
- when using this installation media, the data is installed along with the database structure and **no replication is needed**  
- when would you want to use?
	- network slow
	- network not ready 
- defragmentation
	- 
![[Pasted image 20250206090256.png]]
![[Pasted image 20250206090313.png]]

### Global catalog (GC)
- hosts a **partial attribute** set for other domains in the forest (partial copy of all objects is used for **logon**, **object searches**, and **universal group** membership)
	- e.g. copies just the part needed for login 
		- can improve the login performance
- who tells the computer which server is the GC?
	- DNS record 
- supports **queries** for objects throughout the forest
- pros vs cons
	- pro: improved login performance
	- con: storing more domain footprint 
![[Pasted image 20250206090756.png]]
![[Pasted image 20250206090929.png]]

### Installing and configuring an RODC
- **Read only domain controller (RODC)**
	- contains a full replication of the domain database
- #midtermQ
- can only read objects
- created to be used in places where a domain controller is needed but the **physical security** of the domain controller cannot be guaranteed 
	- e.g. datacenter is protected but not the local branch so you need RODC 
- can improve performance because you have a domain controller near you 
- you can decide which objects will be cached on your RODC 
	- normally won't do full copy of your data
- why use RODC?
	- because having local is faster and you don't need to worry about the network being slow
		- e.g. authentication takes longer without it 
![[Pasted image 20250206091605.png]]
- use pre-create domain controller options 

### Groups
- group objects contain users (from a single or multiple domains or OUs) who require similar access to resources or rights to perform tasks
- members of a group inherits rights and permissions assigned to the group 

#### Group types
- **distribution groups**
	- used only with email applications
	- not security enabled (no SID)
	- cannot be given permissions
		- can't get permissions because no SID
- **security groups**
	- security principal with a SID
	- can be given permissions
	- can also be email enabled
- security groups and distribution groups can be converted to the other type of group 

#### Group scopes
- local groups
	- can contain users, computers, global groups, domain local groups and universal groups from the same domain, domains in the same forest and other trusted domain
	- can be given permissions to resources on the local computer only
- domain local groups
	- includes: 
		- objects (user. computer, printer)
		- domain local (from same domain)
		- universal group 
		- global group
- universal groups
	- includes:
		- objects (e.g. administrator) 
			- enterprise domain admin (most powerful group) 
		- global groups 
		- universal groups 
- global groups 
	- includes:
		- objects
		- other global (from same domain)

##### questions
![[Pasted image 20250206095742.png]]
1. yes
2. no
3. no
4. yes
5. yes
6. no
7. no
8. yes
9. yes
10. yes

#### Implementing group management
- best practice for nesting groups is IGDLA
	- sometimes called IGUDLA to include universal but we don't use it because universal group is too powerful 
- I: identities, users, or computers, which are members of 
- G: Global groups
	- collect members based on members' roles, which are members of
- DL: domain local groups,
	- provide management such as resource access which are
- A: assigned access to a resource  
![[Pasted image 20250206094048.png]]

### HyperV
- what is a hyper visor?
	- Type 1 and Type 2
		- #midtermQ 
- secure boot 
	- UEFI has secure boot but not bios
	- bios firmware confirms the digital key 
	- current (2022 and up) hyperV can support both windows and linux secure boot 
- snapshot
	- 1 type 
- checkpoint is the same as a snapshot 
	- standard checkpoint
		- create differencing disks, .avhd files, which merge back into the previous checkpoint when the checkpoint is deleted
		- record of your RAM 
	- production checkpoint
		- created by using VSS and require starting from an offline state
		- kind of like backup
		- only takes a photo of your disk 
	- what is the difference between them? #midtermQ 
- virtual switch
	- external switch
		- binds with physical hardware
			- e.g. 1 network card means you can make 1 external switch
		- allows them to talk to each other
			- no limitations 
	- internal switch
		- only can talk to each other and their host
	- private switch
		- cannot talk to their host but can talk to each other 
	- virtual hard disk #midtermQ 
		- VHDX supports 64 TB 
		- VHD supports 2TB