## short answer questions
### AD DS
#### what is AD DS?
- Active Directory Domain Services is a specialized type of database called a directory
	- it **stores information** about the identities of users, computers, and services
	- provides **authentication** for users and computers
	- provides **authorization** for users and computers to access network resources 
#### AD DS Partitions
- **domain**
	- includes objects (users, computers, groups, printer)
- **schema**
	- class
		- blueprints, categories
	- attribute
		- used to describe the class
- **application**
	- third parties
		- e.g. DNS records (A, AAAA, SRV, MX, NS)
- **configuration**
	- network infrastructure
	- domain architecture/structure 

#### Domain Controller
- a Domain Controller is a server in AD DS that authenticates and authorizes users and devices within a network domain
- all domain controllers hold a copy of the domain database
- the domain in which user accounts, computer accounts, and groups are created
- the domain is a replication boundary
	- the limit within which directory data is automatically replicated among domain controllers 
- administrative center for configuring and managing objects

#### AD DS physical components and logical components

##### Physical
- **Database (NTDS.dit)**
	- 4 partitions (domain, schema, application, configuration)
- **network authentication protocol**
	- need encryption
	- **Kerberos** is the network authentical protocol
- **DNS**
	- SRV record = who is the domain controller
		- SRV is service location
	- forward lookup zone
		- full qualified domain name to IP lookup
	- reverse lookup zone
		- IP to name lookup
	- DNS port number 53
	- supports TCP and UDP
	- mySQL port 3306
	- sql server port 1433
	- remote desktop port 3389
- **LDAP**
	- communication protocol
	- database follows this to store our data 

##### Logical
- **forest**
	- more than 1 tree
	- collection of domain trees that share a common AD DS
- **tree**
	- more than 1 node or domain
	- your first domain controller is the root of the forest and the root of the domain tree 
		- two way trust exists between parent domains and child domains
		- all domains in a single tree use the same schema for all types of common objects
		- all domains use the same global catalog
- **domains**
	- administrative boundaries for users and computers that are stored in common directory database
- **ou**
	- organizational unit
	- containers in a domain that allow you to organize and group resources for easier administration 

### FISMO
- Flexible Single Master Operations
- special roles in AD DS assigned to domain controllers to handle critical operations by a single DC at a time
	- one computer per role but one computer can play multiple roles
##### Forest wide
- **schema master**
	- one per forest
	- manages the AD schema
		- schema are blueprints(class, attributes)
	- needed for schema modification
	- can be shutdown if not needing to update schema
- **domain naming master**
	- one per forest
	- responsible for ensuring domain names are unique in the forest
	- controls the creation and deletion of domains in the forest 
	- can be shut down if you don't need to name a new sub domain 

##### Domain Wide
- **PDC emulator**
	- cannot be shutdown
	- account manipulation
		- password (modifying account passwords)
	- time synchronization
	- group policy
- **RID Pool Master**
	- assigned unique SID to new objects (users, groups, computers)
	- used in SID
		- last block of SID
	- cannot create security groups or user accounts if you shut this down
- **infrastructure master**
	- reference for how your whole forest is
	- maintains cross domain references for objects 

### RODC 
- Read Only Domain Controller
- can only read objects
- created to be used in places where a domain controller is needed but the physical security of the domain controller can't be guaranteed 
- it could also improve performances because then a domain controller can be closer to you 
- why use RODC?
	- having objects locally is faster than over a network
- how to deploy?
	- Functional Level: Forest functional level must be Windows Server 2003 or higher.
	- at least one writeable DC needed
	- RODC needs to be able to locate a writeable DC via DNS

### IGDLA
 * IGDLA is the best practice for group nesting in AD 
 * structured way to assign permissions to users in Active Directory, helping with scalability, management, and security
 * **I – Individual**
	 * individual users are assigned to global groups based on roles or departments
 * **G – Global Group**
	 * users from the same role are grouped together
 * **D – Domain Local Group**
	 * these groups represent access to specific resources
		 * e.g. HR shared folder access
 * **L – Local Group**  
	 * used on server/workstations if needed
 * **A – Access**
	 * permissions are applied to the Domain Local Group

### how to improve server security?
- strong password
- firewall configuration
- updates and patches
- limit user permissions
- encrypt sensitive data 

### Groups
- group objects contain users (from single or multiple domains or OUs) who require similar access to resources or rights to perform a task
- members of a group inherits rights and permissions assigned to the group

#### Group types
- distribution groups
	- used only with email applications
	- not security enabled
		- no SID
	- cannot be given permissions
- security groups
	- security principle with SID
	- can be given permissions
	- can also be email enabled
- security groups and distribution groups can be converted to the other type of group 

#### Group Scopes
- local groups
	- can contain users, computers, global groups, domain local groups, and universal groups from the same domain, domains in the same forest, and other trusted domains
	- can be given permissions to resources on the local computer only
- domain local groups
	- includes
		- objects
		- domain local, universal group, global group
- universal groups
	- includes
		- objects
		- global groups
		- universal groups
- global groups
	- includes
		- objects
		- other globals (from the same domain)

### Enterprise Storage Solutions 
#### DAS
- direct attaches storage
- available in nearly every server
- advantages
	- each to configure
	- inexpensive
		- no network devices, hubs, switches, or routers 
- disadvantages
	- isolated because the disks are attached to a single server 
	- less flexibility
	- scalability limits 

#### SAN
- storage area network
- enterprise centralized storage solution
	- provides high availability and maximum flexibility 
- allows systems to attach to the storage in the SAN and presents the drives to the server just as if the drives were locally attaches
- advantages:
	- storage can be accessed by multiple servers simultaneously with more robust and faster processing
	- easily expandable
	- centralized storage with high levels of redundancy
		- lots of redundancy due to multiple network devices
- disadvantages
	- most expensive solution
	- requires specialized admin skills 

#### NAS
- network attaches storage
- file level data storage
- connected to the server over a computer network to provide shared drives or folders
- advantages
	- relatively inexpensive centralized storage
	- easy to configure
- disadvantage
	- slower access times
	- not appropriate in many enterprise scenarios 

#### Comparing storage solutions
- DAS
	- least complex
	- low cost set up
- NAS
	- best solution in specific situations
	- often used to compliment DAS and SAN
- SAN
	- highest level of performance
	- provides the most features

### DNS Zones
- **Primary Zone**
	- read'/write copy of a DNS database
	- easy to backup andrecover
	- must be available to make changes
- **Secondary Zone**
	- read only copy of a DNS database
		- can be copies of primary, secondary, and active directory integrated
	- primary zone must be available to make changes
	- windows can be a secondary zone to a unix primary zone
	- can improve performance through load balancing 
	- for security 
- Stub Zone
	- copy of a zone that contains only records used to locate server names
		- redirects requests to a server that can answer it
	- subset of records
		- Glue Host (A) records
		- Start of Authority (SOA)
		- Name Server (NS)
	- using stub zone as a forwarder is better because you don't need to update since it's synched with NS records
- Active Directory Integrated 
	- zone data is stored in AD DS rather than zone files
	- only avilable on domain controllers
	- high availability and redundancy
	- harder to restore in a disaster 

## general study guide

### Windows Server

#### New Features of Windows Server 2016,2019,2022
- 2016
	- added nano servers
	- windows and hyper v containers 
	- shielded VMs
- 2019
	- azure hybrid integration
	- improved security features
	- basic kubernetes support
- 2022
	- secured core server 
	- hot patching

#### how to update/update
| Method               | Description                                                                    |
| -------------------- | ------------------------------------------------------------------------------ |
| **In-place Upgrade** | Keeps existing roles, settings, and data. Good for single-server upgrades.     |
| **Clean Install**    | Fresh start. Wipes everything, best for long-term performance and reliability. |
| **Migration**        | Move roles/data to a new server running the newer OS. Ideal for larger setups. |
- we tend to prefer a fresh install
- when you upgrade, careful of CPU architecture for old ones
	- Newer Windows Server versions may not support old CPU models.
	- Driver incompatibility is common with older hardware
- check your CPU model and confirm compatibility

#### Difference between 2016, 2019, 2022 editions
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

#### Minimum hardware requirements for Windows Server 2016,2019,2022
- process architecture
	- x64
	- processor speed 1.4Ghz
- RAM
	- 512 MB
- hard drive space
	- 32 GB
- for virtual machines:
	- 800 MB RAM

### Types of Accounts in Windows Server
- Client Computer
	- local/standard user
	- admin
	- guest
- Windows Server
	- domain admin
	- domain guest
	- domain user

### What is SID?
- Security Identifier 
	- unique value used to identify users, groups, and computer accounts
	- important for managing security and access control
- role based management uses security groups and that needs SID
- why is it important?
	- consistnecy across name changes
	- access control and permissions
	- authorization and authentication
	- security logs
	- replication and trust in AD
#### Components on SID
- SID follows a standard format: S-R-X-Y1-Y2-...-Yn
- **S - SID Prefix**: Indicates that this is a SID. Always starts with `S`
- **R - Version Number**: Specifies the version of the SID structure
- **X - Identifier Authority**: Represents the entity that issued the SID.
- **Y1, Y2... Yn-1  - Sub-authorities**: Indicates the number of sub-authorities in the SID
- **Yn - Relative Identifier (RID)**: A value that specifies the security principal within the domain or computer.


### How to deploy AD DS
- install AD DS rose
- promote server to a Domain Controller 
- configure DNS

### Why do we need DNS?
- **DNS (Domain Name System)** is critical for AD DS to work properly because:
	- Active Directory **uses DNS to locate domain controllers**
	- Services like **Kerberos, Group Policy, and LDAP** rely on DNS to resolve names (like `dc01.corp.local`) to IP addresses
	- Without a functioning DNS setup, **clients can't join the domain, log in, or find AD resources**

### DNS Levels 
- root level
	- top of the DNS hierarchy
	- `.`
- top level domain TLD
	- e.g. `.com`, `.org`, `.edu`
	- could also include country code TLDs
	- assigned by ICANN
- second level domain
	- the domain name you register beneath the TLD
	- e.g. `example` in `example.com`
- sub domain
	- any domain added before the second level domain 
	- mail.example.com 

### FQDN
- Fully Qualified Domain Name 
- the complete domain name for a specific computer, host, service on the internet (or private network)
- includes all parts of the domain hierarchy making it unique and globally identifiable 
- e.g. `hostname.subdomain.domain.tld`
- host name vs FQDN
	- host name is just the name of the device
	- FQDN is host name plus the full domain path 

### Why do we need LDAP?
- LDAP (Lightweight Directory Access Protocol) is the protocol Active Directory uses to:
	- Access and manage the directory (like users, groups, computers).
	- Handle authentication and querying.
- communication protocol 

### Why do we need a site?
- optimizes network traffic
	- allows AD to optimize replication between domain controllers
	- replication within a site is fast and frequent
- improve authentication efficiency
	- AD uses sites to find the nearest domain controller 
- supports large or multilocation organizations 
	- sites represent physical office location 

### What is global catalogue?
- global catalog is a distributed repository that contains a partial, read-only replica of all objects in the forest 
	- all objects in the directory
	- only a subset of attributes of each object (just enough to search and login)

#### How many gc do you need?
- at least 2 in your forest for redundancy
- one GC is a single point of failure 
- where to place GCs?
	- on every site that has a domain controller
	- domain controller that handle logins for cross domain 

#### What are functional levels in AD?
- functional levels in AD define the set of features and security capabilities available within a domain or forest
- they are determined by the version of Windows Server being used on domain controllers and govern what features and enhancements you can enable within the AD environment
- domain functional level
	- controls the features available for domain controllers within a domain
	- higher functional levels unlock new AD features and improved security mechanisms
- forest functional level
	- governs the features available across all domains in a forest
	- some features are only available at higher forest functional levels 

#### why upgrade functional levels?
- improved security features
- new AD features
- **levels cannot be downgraded ones raised**
- before raising functional level, ensure that all domain controllers are running the minimum requires version of Windows server 
- domain functional levels are raised first, followed by forest functional levels
- raise the domain functional level on each domain before raising the forest functional level
- lowest version first
	- you must start with the lowest version that fits the needs of your domain or forest
	- upgrading functional levels can only be done after all domain controllers are running the required version for the desired functional level
- can't downgrade
- raising functional level may break compatibility with older clients 

### Microserver 
- micro servers are small low power and cost efficient servers designed for handling lightweight or specific tasks 

#### Nano server
- `New-NanoServer-Image`
	- to create a new nano server image
- nano servers are lightweight, minimal installation option for Windows Server designed for small efficient environments like microservers 
##### Nano server usage scenarios and requirements
- no AD DS or DHCP
- can be:
	- file server
	- DNS server
	- web server
	- container

### NTFS
- New Technology File Systems
- permissions control how users and groups can interact with files and folders on NTFS formatted drives

#### Basic Permissions
- full control
- modify
- read and execute
- list folder contents
- read
- write

#### best practice
- assign permission to groups instead of individuals
	- easier to manage
- use least privilege principle 

- domain join 
	- use gui 
	- d join 

### IPV4 and IPV6 
- these are the IP addresses so that they can send and receive data
- ipv4
	- written as four decimal octets seprated by dots 
	- `192.168.`
- ipv6
	- written as 8 groups of 4 hexadecimal digits seperated by :
	- `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

### Storage Pool
- storage pools allow you to group together multiple physical disks into a single logical storage unit 

#### Creating storage pools
- 1 physical disk to make a storage pool
- at least 2 physical disks are required to create a resilient, mirrored virtual disk 
- at least 3 physical disks are required to create a virtual disk with resilience through parity 
- at least 5 physical dsisk are required for three way mirroring 
- at least 7 physical disks are required for dual parity
- disks must be blank and unformatted, which means no volumes can exist on the disks
- disks can be attached using a variety of bus interfaces including SAS, SATA, SCSI, NVMe, USB

#### S2D
- Storage Spaces Direct
- enables the creation of software defined storage by pooling together storage from multiple servers and presenting it in a singl storage pool
- no need for shared storage
- supports failover clustering 
- scalable
- works with hyper v
- 

#### RAID 
- RAID 0
	- striped
- RAID 1
	- mirrored
- RAID 5
	- striped with parity
- RAID 6
	- striped with 2 parity
- RAID 1 + 0
	- mirrored and striped 


### GPO
- group policy object
- allows administrators to centrally manage and configure operating systems, applications, and user settings in an active directory environment 

#### What is GPO
- controls settings for users and computers
- can apply security policies, software installations, desktop settings, etc. 
- automatically enforces settings on computers and users when they log in or start up 

#### Configuring security policies with GPO
- configure user rights assignment
	- controls which users/groups can perform system level tasks (e.g. log on locally, shut down the system, access from then network)
	- `GPO > Computer Configuration > Windows Settings > Security Settings > Local Policies > User Rights Assignment`
- configure security options settings
	- fine tune system behaviours and security rules
	- `GPO > Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`
- use security templates 
	- predefined sets of security configruations that can be applied to systems
	- use security template snap in or import into a GPO via security configuration and analysis
	- purpose:
		- ensures consistency across multiple machines
	- common templates:
		- securews.inf, hisecdc.inf 
- configure audit policy
	- logs key system and user event for monitoring and investigation
	- `GPO > Computer Configuration > Windows Settings > Security Settings > Local Policies > Audit Polic`
	- you can audit:
		- logon events
		- file access
		- privilege use
		- system events 

### LSDOU
- key concept in GPO application and explains the order of GPO processing
- local -> site -> domain -> OU
- L - local
	- local group policy applied first
	- default policy on every windows machine
- S - site
	- next GPOs linked to the AD are applied
	- sites represent physical locations in AD
- D- domain
	- then GPOs linked at the domain level are applied
	- often include global seucirty policies 
- OU - organizational unit
	- GPOs linked to the OU that the object belongs to are applied
	- OU GPOs override previous ones if there is a conflict 
- last applied overrides earlier ones unless enforced
- you can block inheritance or use enforce to override blocking


### DORA in DHCP
- DORA is the 4 step process that a device uses to get an IP address from a DHCP server
- D - Discover
	- the client broadcasts looking for any DHCP server
	- sent as a broadcast msg
- O - offer
	- DHCP server responds with an IP address offer
- R - request
	- client requests to use the IP and other config info
- A - acknowledge 
	- DHCP server confirms and leases the IP

#### Lease renewal timings
- at 50% of the lease duration, the client tries to renew the lease directly with the original DHCP
- at 87.5%, if renewal failed, the client broadcasts to any DHCP server for a lease extension 
- default lease values
	- lease duration 8 days
	- 50% - 4 days
	- 87.5% - 7 days 


### DNS 
- Domain Name System
- translates domain names into IP addresses 

#### Commons DNS records
- A
	- ipv4 address
- AAAA
	- ipv6 address
- MX
	- mail exchange
- CNAME
	- canonical name' alias for another domain 
- NS
	- name server
	- specified authoritative DNS servers for a domain
- SOA
	- start of authority
	- contains domain level info like primary name server, admin email, and update times 
- SRV
	- service locator 
	- specifies location of services like SIP, Kerberos, etc.
- TXT
	- text
	- holds arbitrary text 
	- often used for domain verification or policy settings
- SPF
	- sender policy framework
	- special kind of TXT record used to preven email spoofing by listing allowed mail servers 

### Virtualization
- virtualization is creating virtual versions of computing resources like OS, servers, storage devices, or networks 
- type 1
	- bare metal hypervisor
	- directly on hardare
	- high performance
	- used in data centers or production servers
- type 2
	- hosted hypervisor
	- on top of host OS
	- slightly lower performance due to overhead
	- used for personal use and testing
	- e.g. VMware, VirtualBox 

#### Hyper V generations
- gen 1
	- legacy BIOS
	- older OS support
	- basic security
- gen 2
	- UEFI 
	- modern OS only (WIndows 8+)
	- better boot speed, secure boot 

#### Switches in Hyper V
- External
	- Connects VMs to the physical network (and internet)
- Internal
	- communications between VMs and the host machine only
- Private
	- communication between VMs only
	- no host or external access

#### Container
- a container is a lightweight standalone package that includes everything needed to run a piece of software
- unlike VMs, containers share the host OS kernel 
- Container is like apartment units in a building (shared structure) and VMs are separate houses

#### Checkpoints
- standard/production checkpoints
	- captures application consistent state
	- safe and cleader for databases and apps
	- good for production sysmtes
- classic checkpoints 
	- useful in development/testing environments
	- captures the full VM state
		- includes: memory, CPU state, Disk
	- not ideal for running services or devices

### VPN tunneling protocols
- which port for which protocols 
	- PPTP
		- TCP 1723
		- easy to set up but outdates and not secure
	- L2TP
		- UDP 500, 1701, 4500
		- uses IPsec for encryption 
		- more secure than PPTP but can be blocked by NAT/firewalls 
	- SSTP
		- TCP 443
		- tunnels over HTTPs
		- good to bypassing firewalls and NAT
	- IKv2
		- UDP 500, 4500
		- stable on mobile networks 
		- good for phones/laptops 
- PPTP is not a good tunneling protocol because of weak encryption 
- Best Practices
	- **Use IKEv2/IPsec** for strong security and fast reconnects on mobile
	- **Use SSTP** when behind strict firewalls or on corporate networks (thanks to port 443).
	- **Avoid PPTP** unless for legacy systems/testing only.

### configuring authentication options
- CHAP
	- challenge handshake authentication protocol
	- an authentication method where the server sends a challenge to the client, and the client responds with a hash of their password and the challenge
	- used for PPP (point to point protocol) and VPN connections 
	- still weak by modern standards 
- EAP
	- extensible authentication protocol
	- a framework that supports multiple authentication methods
	- used for wired/wireless networks, VPNs
	- strongest because:
		- supports certificate based authentication
		- can use biometrics, tokens, smart cards 
		- often used in enterprise wireless security 

