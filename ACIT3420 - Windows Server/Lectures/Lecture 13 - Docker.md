### what is nano server?
- extremely small server with limited features and functions
	- it can be a DNS, fail over server, web server, file server, etc. 
	- can't do things like AD DS or DHCP
- nano server is a new installation option got windows server 2016
- only supports 64 bit applications, tools, and agents 
- different from windows server core, you must creste a virtual hard disk
	- then you can use the virtual hard drive on a virtual machine to support a virtualized nano server in Hyper V 
	- or you can have the nano server start from a .vhd file
- why use nano server as a web server?
	- uses less resources 
	- less than 550 mb 
- can use nano server as a container 

#### benefits of nano server
- improved security (base install 12 vs full windows gui 34)
- lowered costs
- faster boot times
- easier remote management
- smaller server image 

#### usage scenarios and requirements
- #finalQ
- no AD DS or DHCP
- can be:
	- file server
	- DNS server
	- web server
	- container

#### nano server installation step
- `New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath E:\ -  BasePath c:\nano -TargetPath c:\nano\CA-VAN-Nano-Svr1.vhdx -Storage -Package  Microsoft-NanoServer-IIS-Package -ComputerName Nano-Svr1`
- know `New-NanoServer-Image`
	- to create a new nano server image
	- #finalQ
- DeploymentType - guest = for VM
	- could do it for host machine if you want
- Edition - standard or data center 

#### hyperV virtual switches
#finalQ 

#### checkpoints
#finalQ 

### general study guide
- windows server
	- how to update, upgrade
	- when to update and upgrade
	- prefer fresh install
	- when you upgrade, careful of CPU architecture for old ones
- new features of windows 16,19, 22
	- 2025 gets hot patching and hyper cloud
		- can use windows server api or VPN
- 16,19,22 understand the different editions
	- standard, data center, enterprise
		- what are the minim requirements
- diff types of accs in windows server?
	- admin or guest acc for server
	- client computer has local user, admin, guest (think im missing one)
- SID
	- what makes up SID and why do we need?
	- role based management uses security group and that needs SID
- what is domain controller
- how to deploy AD DS?
- domain components
	- physical and logical
- why do we need DNS?
- what is LDAP?
- why do we need a site?
- what is global catalogue?
	- how many do you need to prepare? at least 2 
	- only one is a single point of failure
- what is functional levels?
	- security and features
	- you look at lowest addition first
		- need to upgrade before you raise
- FQDN
	- what is it?
- DNS
	- top level vs second level
- micro server
	- which command line to make new nano image
- domain join 
	- use gui 
	- d join 
- NTFS basic permission
- how to assign appropriate NTFS permission? 
- ipv4 vs ipv6
	- know the format of ipv6
- what is storage pool?
	- s2d
	- enterprise storage solution
		- understand das, nas, san
	- different layouts
		- how many disks are required for different parities
- RAID
	- know the RAIDS
- what is GPO?
- configuring security policies with GPO
- make sure you can do
	- configure user rights assignment
	- security options settings
	- security templates
	- audit policy 
- LSDOU
- understand DORA for dhcp stuff
	- 50 and 87.5
	- default values for DHCP
- DHCP can reserve IP
- what is DNS
	- DNS records
		- SRV, SOA, etc.
		- mx records, txt, spf,
	- what are the DNS zones?
		- primary zone, secondary zone, stub zone, AD integrated zone
- virtualization
	- type 1 vs  type 2
	- hyper V
		- gen 1 vs gen 2
		- 3 types of switches
	- what is a container 
		- what is the difference 
	- checkpoints 
- VPN
	- protocol and the ports
	- VPN tunneling protocol and the require port 
### short answer questions
- #finalQ 
- definition of AD DS and partition
- FISMO
	- list all roles and explain
- RODC and why do we need?
	- how do we deploy?
- IGDLA
	- scope
- how to improve server security?
	- strong password,
- domain local group, global groip, universal goup, distributon and nesting group
- enterprise storage solution
	- understand das, nas, san
	- das or san for block level storage for an enterprise 
- what are the DNS zones?
	- primary zone, secondary zone, stub zone, AD integrated zone
		- secondary zone can get data from primary or secondary zone
		- stub zone only has 3 types of DNS records
			- NS records, SOE records, glue A
			- like a forwarder
			- primary zone is the forwarder