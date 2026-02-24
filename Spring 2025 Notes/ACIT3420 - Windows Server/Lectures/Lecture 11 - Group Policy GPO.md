### Group policy
- group policy is settings or rules for specific groups 
- mechanism for controlling and deploying operating system settings to computers all over your network
- provides centralized control for user settings and computer settings
- controls user experience 
- control and deployment of applications 

### Types of GPO (group policy object)
- **local GPOS (GPEDIT.MSC)**
	- on the local computer only
- **domain GPS (GPMC and GPME)**
	- created in Active Directory
- **starter GPOs**
	- template GPO based on a standard collection of settings
	- these objects enable an administrator to create and have a pre-configured group of settings that represents a baseline for any future policy to be created
	- template of template 

### Group Policy
- consists of user and computer settings for the various windows OS
- implemented during computer start up and shut down and user logon and logoff
- configure one or more GPOs and then use a process called linking to associate them with specific AD DS objects
- policy vs preferences
	- #finalQ 
- computer settings vs user settings
	- #finalQ 
	- if computer and user settings are conflicting, computer settings always wins 

### characteristics of Group Policy 
- group policy and be set for site, domain, ou, or local computers
- group policy cannot be set for non-ou folder containers
- group policy settings are stored in GPOs
- group policy can be set up to affect user accounts and computers
- when group policy is updated, old policies are removed or updated for all clients

#### benefits of using GPO
- powerful admin tool
- use it to enforce various types of settings to a large number of users and computers
- typically use GPO to
	- apply security settings (firewall)
	- manage desktop application settings
	- deploy application software (WDS vs GPOs)
		- we can push application to the client through internet if they're connected to the cloud 
	- manage folder redirection
		- kind of outdated now 
		- can redirect folders to the server side 
		- same as one drive so people don't use folder redirection anymore 
	- configure network settings 

### why group policy is important?
- text based configuration file
	- difficult to keep update to date
- registry
	- changes cannot be rolled back
- group policy
	- centralized management 

### limitations of GPO
- #finalQ 
- they run sequentially
	- GPOs process actions one after another
- flexibility is limited
- limited triggers (computer startup or user logs on or at set intervals)
	- some policies may need you to reboot too 
- difficult to maintain
- no version control
	- changes made to GPO settings aren't audited 
- relies on network speed for deployment
	- if network is slow, it could cause some GPO to not be deployed 
- only deployed when a computer is turned on/rebooted/re logged 

### group policy processing
- LSDOU #finalQ 
	- processes GPOs in the following order 
	- **local policies** (executed first)
	- **site policies**
	- **domain policies**
	- **OU policies** (executed last)
	- within the policies, last one applied wins
![[Pasted image 20250320095924.png]]

### common GPO management tasks
- backup GPOs
- restore GPOs
- copy GPOs
- import GPOs

### WMI
#finalQ 
`- SELECT * FROM Win32_Battery WHERE (BatteryStatus <>  0)`
`- SELECT * FROM Win32_ComputerSystem WHERE PCSystemType = "2"`
- production types
	- 1
	- 3 =