### Windows Server Licenses
#### Difference between standard, essential, data center windows server 2022
- **essential**
	- small business
	- intended for small businesses with up to 25 users and 50 devices
- **standard**
	- primarily for physical or minimally virtualized environments
	- use this if you have a local branch
	- supports up to 2 hyper V
		- can technically do more but you need to buy licenses
	- AD DS will still use up one service if not on the hyper V
- **datacenter**
	- designed for highly virtualized datacenters and cloud environments
	- supports unlimited instances of hyper V
- **datacenter - azure edition**
	- hot patch
	- SMB over QUIZ
	- extended network for azure 


#### what is the current Microsoft windows server licensing strategy? 
- windows server 2019 and up, its based on how many cores
	- 1 pack supports up to 2 CPU or 16 cores 
- older ones were based on physical CPUs
- need another additional type of license for services 
	- CAL license

#### Minimum hardware requirements for Windows Server 2016,2019,2022
- process architecture
	- x64
- processor speed
	- 1.4Ghz
- RAM
	- 512 MB
- hard drive space
	- 32 GB
- for virtual machines:
	- 800MB RAM

### ADDS  
- **Active Directory Domain Services (AD DS)** is a specialized type of database called a directory
	- **stores information** about the identities of users, computers, and services
	- provides **authentication** for users and computers
	- provides **authorization** for users and computers to access network resources 

#### What is an AD DS?
- AD DS requires one or more domain controllers
	- A **Domain Controller (DC)** is a **server** in Active Directory Domain Services (AD DS) that **authenticates and authorizes** users and devices within a network domain
- all domain controllers hold a copy of the domain database
	- they are continually synchronized
- the domain is the context which user accounts, computer accounts, and groups are created
- the domain is a **replication boundary** 
	- the **limit** within which **directory data is automatically replicated** among domain controllers
- the domain is an administrative center for configuring and managing objects 
	- e.g. changing passwords
- any domain controller can authenticate any sign in anywhere in the domain

### AD DS Partitions

<mark style="background: #ABF7F7A6;">database (NTDS.dit)</mark>
	- 4 partitions
		- **domain**
			- includes objects (users, computers, groups, printer)
		- **schema** 
			- class
				- blueprint, category 
			- attribute 
				- used to describe the class
		- **application**
			- third parties 
				- e.g. DNS records (A, AAAA, SRV, MX, NS)
					- A is ipv4, AAAA is ipv6, MX is mail, SRV is who is domain controller 
		- **configuration** 
			- network infrastructure 
			- domain architecture/structure 


### ADDS physical components and logical components
#### Physical
- <mark style="background: #ABF7F7A6;">database (NTDS.dit)</mark>
	- 4 partitions
		- **domain**
			- includes objects (users, computers, groups, printer)
		- **schema** 
			- class
				- blueprint, category 
			- attribute 
				- used to describe the class
		- **application**
			- third parties 
				- e.g. DNS records (A, AAAA, SRV, MX, NS)
					- A is ipv4, AAAA is ipv6, MX is mail, SRV is who is domain controller 
		- **configuration** 
			- network infrastructure 
			- domain architecture/structure 
- <mark style="background: #ABF7F7A6;">network authentication protocol </mark>
	- need encryption 
	- **Kerbos** is network authentication protocol 
- <mark style="background: #ABF7F7A6;">DNS</mark>
	- SRV record = who is domain controller
		- SRV is service location 
	- forward lookup zone
		- full qualified domain name to IP lookup
	- reverse lookup zone
		- IP to name lookup 
	- **DNS port number 53**
	- support TCP and UDP
	- **remote desktop port number 3389** 
	- **mySQL port number 3306** 
	- **Microsoft sql server port number 1433** 
- <mark style="background: #ABF7F7A6;">LDAP</mark>
	- communication protocol
	- NTDS.dit follows this to store our data 
	- for queries 

#### Logical 
- **forest**
	- more than 1 tree
	- collection of domain trees that share a common AD DS
- **tree**
	- more than 1 node or domain 
	- your **first domain** controller is the root of the forest and the **root of the domain tree** 
		- two way trust relationship exists between parent domains and child domains
		- all domains in a single tree use the same schema for all types of common objects
		- all domains use the same global catalog 
	- collections of domains are grouped together in a hierarchical structure and share a common root domain
- **site**
	- group of domain controllers 
	- for replication purpose 
- **ou**
	- organizational unit
	- how many you need is based on your company architecture
		- e.g. how many departments you have 
	- containers in a domain that allow you to organize and group resources for easier admin 

### Sites
- A TCP/IP-based concept (container) within Active Directory that is linked to IP subnets
- A site has the following functions:  
	- Reflects one or more interconnected subnets  
	- Reflects the physical aspect of the network  
	- Is used for DC replication  
	- Is used to enable a client to access the DC that is physically closest 
	- Is composed of only two types of objects, servers and configuration objects
- for **replication** 
	- we want to use a site so that all the servers don't need to talk to each other
- only the "reps" should replicate with each other
	- the other servers will replicate to the rep
- **bridgehead** = rep
- you want to have this to limit the amount needing to be replicated across different sites
	- that way only 1 server needs to send the info rather than all of them 
- know how to replicate a site

### FSMO
- Flexible Single Master Operations (FSMO)
- special roles in AD DS assigned to domain controllers to handle critical operating by a single DC at a time
- one computer per role but one computer can play multiple roles
#### Forest wide
- **schema master**
	- one per forest
	- manages the AD schema
		- schemes are blueprints (class, attributes)
	- needed for schema modifications 
	- can be shut down if you don't need to update the schema
- **domain naming master**
	- one per forest
	- responsible for ensuring domain names are unique in the forest
		- e.g. if you're adding a sub domain, you can't add another sub domain with the same name
	- controls the creation and deletion of domains in the forest 
	- can be shut down if you don't need to name a new sub domain
	- two way trust
		- build 1 way first trust one way then build it the other way 

#### Domain wide
- **PDC emulator** 
	- cannot shutdown
	- account manipulation
		- password
			- modifying account passwords 
	- time synchronization 
	- group policy
		- needed for deploying policy 
- **RID pool master**
	- assigns unique SID to new objects (users, groups, computers)
	- SID 
		- the last block of SID is RID 
	- cannot create security groups or user accounts if you shut this down 
- **infrastructure master**
	- reference for how your whole forest is 
		- anchor point
	- maintains cross-domain references for objects 

### Global Catalog (GC)
- hosts a partial attribute set for other domains in the forest
	- the partial copy of all objects used for logon, object searches, and universal group membership
- supports queries for objects throughout the forest 
- a read only index that stores a subset of directory objects from all domains in the forest 

### RODC
- Read Only Domain Controller (RODC)
	- contains a full replication of the domain database 
- can only read objects 
- created to be used in places where a domain controller is needed but the physical security of the domain controller can't be guaranteed
	- e.g. datacenter is protected but not the local branch so you use an RODC
- could improve performance because you can have a domain controller near you
- you can decide which objects will be cached on your RODC
- why use RODC?
	- having the objects locally is faster than over a network 

### Groups
- group objects contain users (from single or multiple domains or OUs) who require similar access to resources or rights to perform a task
- members of a group inherits rights and permissions assigned to the group

#### Group types
- **distribution groups**
	- used only with email applications
	- not security enabled
		- no SID
	- cannot be given permissions
- **security groups**
	- security principal with SID
	- can be given permissions
	- can also be email enabled
- security groups and distribution groups can be converted to the other type of group

#### Group scopes
- local groups
	- can contain users, computers, global groups, domain local groups and universal groups from the same domain, domains in the same forest and other trusted domains
	- can be given permissions to resources on the local computer only
- domain local groups
	- includes
		- objects (users, computers, printers)
		- domain local (from the same domain)
		- universal group
		- global group
- universal groups
	- includes
		- objects (e.g. admin)
			- enterprise domain admin (most powerful group)
		- global groups
		- universal groups
- global groups
	- includes
		- objects 
		- other globals (from the same domain)

### IGDLA  
- IGDLA is a best practice for group nesting in AD 
- **I - Identities** (users)
	- individual users are assigned to global groups based on roles or departments
- **G - Global Groups**
	- users from the same role are grouped together
- **D - Domain Local Groups**
	- these groups represent access to specific resources
		- e.g. HR shared folder access
- **L - Local Groups** (on resources)
	- used on server/workstations if needed
- **A - Access** (Permissions)
	- permissions are applied to the Domain Local Group 

### Hyper-V; Hypervisor, T1 vs. T2
- **Hyper V** is a virtualization platform to run multiple virtual machines on a single physical host 
	- it is a type 1 hyper visor
- **Hypervisor** is a software that creates and manages virtual machines 

#### Type 1 Hypervisor (Bare Metal)
- runs directly on physical hardware (bare metal)
- provides better performance and security
- used in enterprise environments and cloud data centers 

#### Type 2 (Hosted)
- runs on top of an existing OS
- easier to install but slower than Type 1 due to overhead
- best for personal use, testing, and development
- e.g. VMware, Virtual Vox

### Internal vs External vs Private switches; Checkpoint;  
- hyperV virtual switches allow VMs to communicate with each other, host machine, or external networks

#### External switch
- connects to VMs to the physical network and the internet
- allows communication between VMs, the host, and external devices
- requires a physical network adapter to the hsot



### iSCSI SAN
- network based storage solution that allows servers to access and manage storage devices over an IP network
- uses iSCSI protocol 
- components:
	- IP network
	- iSCSI targets
		- servers that run on the storage device
	- iSCSI initiators
		- software or host adapter that provides access to targets
	- IQN
		- globally unique identifier used to address initiators and targets on an iSCI network

#### iSCSI target server
- available as a role service in Windows server
- provides different types of functionality 
	- network or diskless boot
	- server application storage
	- non-microsoft platforms
	- lab environments
- features
	- authentication
	- query initiator 
	- virtual hard disks
	- scalability and manageability 

#### ISCSI initiator
- runs as an operating system service
- installed by default in Windows server 2008 and up
- only requires the service to be start before configuring the initiator to connect to iSCSI target 


### Storage and Storage Pools
- a physical hard drive can be designated a basic disk or a dynamic disk
	- basic disk
		- default storage for Windows OS
		- contains only simple volumes
	- dynamic disk
		- can be modified without restarting the Windows system
		- can contain simple, spanned, striped, and mirrored volumes
- a partition is a defined storage space on a hard disk
	- a hard disk needs to have at least 1 partition
- a volume is a partition that has been formatted into a file system
	- requires a system volume and a boot volume

### RAID
- combines multiple disks into a single logical unit to provide fault tolerance and performance
- dynamic disk volumes
	- simple
		- uses free space available on a single disk
	- spanned
		- extends a simple volume across m
	- mirrored
		- duplicates fata from one disk to a second for fault tolerance and redundancy 
	- striped
		- stores data across two or more physical disks
		- data on a striped volume is written evenly to each of the physical disks in the volume

#### Raid 0
- striped without parity or mirroring
![[Pasted image 20250224153759.png]]

#### Raid 1
- mirrored drives
![[Pasted image 20250224153821.png]]

#### Raid 5
- Block level striped set with parity distributed across all disks
![[Pasted image 20250224153949.png]]

#### Raid 6
![[Pasted image 20250224154152.png]]

#### Raid 1+0
- each pair of disks is mirrored and then mirror disks are striped
![[Pasted image 20250224154217.png]]

### Enterprise Storage Solutions (DAS, NAS, SAN) 

#### Direct Attached Storage (DAS)
- available in nearly every server
- can include:
	- physical disks located in the server
	- connections to an external array
	- connections to USB disks
- advantages
	- easy to configure
	- inexpensive 
		- no network devices, hubs, switches, or routers
- disadvantages
	- isolated because the disks are attached to a single server
	- less flexibility
	- scalability limits
![[Pasted image 20250224155134.png]]

#### Network Attached Storage (NAS)
- **file level** data storage
- connected to the server over a computer network to provide shared drives or folders
- usually using Server Message Block (SMB) or Network File System (NFS)
- storage locations are accessed through file level access to shares
- devices are often in headless configuration requiring network communication in order to configure the device 
- advantages:
	- relatively inexpensive centralized storage
	- easy to configure
- disadvantage:
	- slower access times
	- not appropriate in many enterprises scenarios (sql, exchange)
![[Pasted image 20250224155801.png]]

#### Storage Area Network (SAN)
- enterprise centralized storage solution
	- provides high availability and maximum flexibility 
- allows systems to attach to the storage in the SAN and presents the drives to the server just as if the drives were locally attached
- **block level** access to centralized storage
- advantages:
	- storage can be accessed by multiple servers simultaneously with more robust, faster processing 
	- easily expandable 
	- centralized storage with high levels of redundancy 
		- lots of redundancy due to multiple net work devices
- disadvantages:
	- most expensive solution
	- requires specialized administrative skills
- protocol to configure SAN: Fiber Channel
	- fiber channel is a high performance network technology primarily used within SAN solutions 
	- node: any device connected to the SAN
	- WWN (world wide name): unique identifier used to identify storage devices
	- Fabric: encompasses all hardware that connects servers and workstations to storage devices through the user of fiber channel switching technology 
![[Pasted image 20250224161654.png]]

#### Comparing storage solutions
- DAS
	- least complex
	- low set up costs
- NAS
	-  best solution in specific situations
	- often used to compliment DAS and SAN solutions
- SAN
	- highest level of performance
	- provides the most features


### SID
- unique string of numbers assigned to every user, group, or computer in Windows and AD
- serves as the identity of an object within the system
	- even if the object name changes
- why is it important?
	- consistency across name changes
	- access control and permissions
	- authorization and authentication
	- security logs 
	- replication and trust in AD

#### Components of a SID
- SID follows a standard format: S-R-X-Y1-Y2-...-Yn
- **S - SID Prefix**: Indicates that this is a SID. Always starts with `S`
- **R - Version Number**: Specifies the version of the SID structure
- **X - Identifier Authority**: Represents the entity that issued the SID.
- **Y1, Y2... Yn-1  - Sub-authorities**: Indicates the number of sub-authorities in the SID
- **Yn - Relative Identifier (RID)**: A value that specifies the security principal within the domain or computer.