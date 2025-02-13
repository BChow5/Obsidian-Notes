- what is a hyper visor?
	- virtual machine manager or virtual machine monitor 
	- what is the difference between type 1 and type 2?
		- type 1 
			- ESXI
			- gives better performance
			- bare metal
			- on the hardware 
			- for production environment 
		- type 2
			- OS in OS
			- e.g. vmware is type 2 because we install our vmware onto our windows 11 
			- for lab environment 
	- #midtermQ 
- difference between standard, essential, data center windows server
	- #midtermQ
- what is the current Microsoft windows server licensing strategy? 
	- windows server 2019 and up, its based on how many cores
		- 1 pack supports up to 2 CPU or 16 cores 
	- older ones were based on physical CPUs
	- need another additional type of license for services 
		- CAL license
	- #midtermQ 

### NTFS benefits vs FAT32 
- compression
- security
- disk quota

### SID
- Security Identifier 
	- need to be able to list the 5 components of a SID
	- #midtermQ
- we have an SID, and each SID can have permissions
- SIDs are unique 
- each group also have SID

### Why upgrade from 2019 to 2022 windows server?
- security
	- lots of improvements to network security
	- supports TPM 2.0 
- hybrid cloud 
	- much easier to build hybrid cloud 
	- Microsoft has an API for it
- virtualization improvements 

#### Azure hybrid capabilities
- azure arc enabled windows server
	- brings on premises and multicloud servers to azure
	- consistent with native azure VMs
	- treated as a resource in azure 
- windows admin center improvements
- azure auto manage hot patch 
	- new way to install updates
	- doesn't require a reboot after installation

### Windows Server 2022 Edition
- essentials
	- small business
	- intended for small businesses with up to 25 users and 50 devices 
- standard
	- designed primarily for physical or minimally virtualized environments 
	- use this if you have a local branch 
	- supports up to 2 hyper V 
		- can technically do more but you need to buy more licenses 
	- things like ADDS will still use up one service if its not on the hyper V 
- datacenter
	- designed for highly virtualized datacenters and cloud environments 
	- supports unlimited instances of hyper V
- datacenter - azure edition
	- hotpatch
	- SMB over QUIC
	- extended network for Azure 

### CAL
- buy them in Microsoft volume licensing service center 

### Minimum hardware requirements for Window Server 2016,2019,2022
- Windows server 2016/1029/2022 have the following minimum hardware requirements for **server core** installation 
- process architecture
	- x64
- processor speed
	- 1.4GHz
- RAM
	- 512 MB
- hard drive space
	- 32 GB
- note: VM RAM 800 MB

### Windows Server Servicing Branches
- long term servicing channel (LTSC)
	- up to 10 years
- semi annual channel (SAC)
	- no longer being used 