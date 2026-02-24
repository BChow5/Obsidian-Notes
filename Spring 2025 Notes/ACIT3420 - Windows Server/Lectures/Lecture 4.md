### What is an AD DS?
- AD DS requires one or more domain controllers
	- one domain should have at least 2 controllers 
	- The two physical elements of Active Directory are **domain controllers** and **sites**
	- #midtermQ
- AD DS stands for Active Directory Domain Services. It's a Microsoft service that helps manage and organize network resources, such as users, computers, and other devices. 
	- It stores directory data like user accounts, passwords, and permissions, and provides authentication and authorization services to allow secure access to these resources in a domain
- all domain controllers hold a copy of the domain data base, which is **continually synchronized** 
- the domain is the context within which user accounts, computer accounts, and groups are created
- the domain is  **replication boundary**
	- databases will replicate with each other
	- domain is also **security boundary** 
		- can deploy group policy to the same domain 
- you can log into your sub domain computer but not the subdomain controller using a domain user account
- two way trust 
- the domain is an administrative center for configuring and managing objects (e.g. change password)
- any domain controller can authenticate any sign in anywhere in the domain
- the domain provides authorization 
- why do we want multiple domain controllers?
	- fault tolerance
	- load balancing 
- kerbos is network authentication 
	- port 88 
	- DNS uses round robin by default for the load balancing 
- popular load balancing methods
	- URL hash, IP hash, round robin
	- depends on your service infrastructure 
- SQS Amazon Simple Queue Service
	- used if you have a lot of clients and you don't want them wait so you put them into the queue
		- e.g. buying an item, we let them buy but we give them a confirmation email later after they're done the queue rather than waiting for the process themselves 

### Active Directory Basics
- domain controllers (DCs)
	- servers that have the AD DS server role installed
	- contain writable copies of information in Active Directory
- member servers
	- servers on a network managed by Active Directory that do not have Active Directory installed
- domain
	- container that holds information about all network resources that are grouped within it
	- every resource is called an object


### FMSO
- what are the five roles
- what do they do?
- which is forest and domain level
- how to see who is what role using the cli 
- one computer per role but one computer can play multiple roles 
- #midtermQ 

#### Forest wide
- **schema master**
	- one per forest
	- schemes are blueprints (class, attributes)
	- can be shut down if you don't need to update the schema
- **domain naming master**
	- one per forest
	- responsible for the names 
	- e.g. if you're adding a sub domain, you can't add another sub domain with the same name
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
	- SID 
		- the last block of SID is RID 
	- cannot create security groups or user accounts if you shut this down 
- **infrastructure master**
	- reference for how your whole forest is 
		- anchor point

### Function level
- new features 
- security
- functional levels are designed to provide backwards compatibility in AD DS installations with domain controllers running various versions of the Windows Server operating system
- by selecting the functional level representing the oldest version running on your domain controllers, you disable the new features so that the various domain controllers can interoperate properly 
- impossible to lower the function level back down 
- functional level can also be raised for forests 

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
- bridgehead = rep
- you want to have this to limit the amount needing to be replicated across different sites
	- that way only 1 server needs to send the info rather than all of them 
- know how to replicate a site

### DNS records
- kerbos 
- password
- LDAP 

### Quiz stuff
- what is domain service and why is it important?
- what is the database and partitions
- what are the physical and logical components?
- FMSO roles and what they do
- #midtermQ
