### incident identification
#finalQ
![[Pasted image 20260130091341.png]]
- **security events**
	- any observable occurrence within a system or network
	- not necessarily harmful. could be routine actions like a user login or file access
	- only flagged if they deviate from the norm or are of interest in security monitoring
- **adverse security events**
	- events that negatively affect the security of systems or data
	- these might not always lead to significant harm but indicate potential issues, such as failed login attempts or suspicious network traffic 
- **security incidents**
	- a confirmed event or series of adverse events that result in a breach of security policy such as data theft or unauthorized access
	- incidents require immediate action as they represent a tangible threat to the confidentiality, integrity, or availability of assets 

### incident data sources
- IDS/IPS
	- intrusion detection system
	- intrusion prevention system
- firewalls
- authentication systems
- integirty monitoring
- vulnerability scanners
- system event logs
- netflow records
- anti-malware packages
![[Pasted image 20260130094135.png]]

### SIEM (security information and event management)
- #finalQ 
- solution that helps orgs detect, analyze, respond to security threats before they harm business operations
	- can act as firewall, sandboxing, IDP/IDS
	- all packaged into one
- centralized log management
	- all system send logs to the SIEM platform
- ai driven threat detection
	- correlate and analyze log in data that could indicate a malicious activity 

### syslog
- standard for sending and receiving log messages between applications, systems and devices on linux
![[Pasted image 20260130094442.png]]
- mnemonic is a short code specifying an event
- severity 
	- 0 - emergency
	- 1 - alert
	- 2- critical
	- 3- error
	- 4- warning
	- 5 - notification
	- 6 - info
	- 7 - debugger

### determining incident security
- assessments based on the CIA triad
- confidentiality - does the incident expose sensitive info to unauthorized parties?
- integrity - are there alterations or potential damage to the accuracy and completeness of data
- availability - is there any disruption in access to critical systems or data?

### incident severity levels
- SEV 1 - critical incident that affects a large number of users in production
- SEV 2 - significant problem affecting a limited number of users in production
- SEV 3 - incident causes errors, minor problems for users, or heavy system load
- SEV 4 - minor problem that affects the service but doesn't have serious impact
- SEV 5 - a low level deficiency that causes minor problems 

#### Data sensitivity focus
- PII - personal identifiable information
- PHI - personal health information
- SPI - sensitive personal info (generic or sexual orientation data)
- PCI - payment card industry data

#### security assessment criteria
- downtime, recovery time, data integrity breach, economic impact, business processes critically
- adapt criteria to specific needs and environment
- apply consistent criteria for effective incident management and resource allocation 
![[Pasted image 20260130100012.png]]

### NIST incident response
![[Pasted image 20260130100044.png]]

### choosing a containment strategy
- potential damage to and theft of resources
- need for evidence preservation
- service availability
- time and resources needed to implement strategy
- effectiveness of strategy
	- partial or full containment
- duration of the solution
	- e.g. emergency work around to be removed in 4 hours, temporary workaround to be removed in 2 weeks, permanent solution

### containment techniques
- segmentation - dividing networks into logical segments
- isolation - move compromised systems to a network completely disconnected from the main network
- removal - completely disconnects impacted systems from any network to cut off the attacker's access 

### incident eradication and recovery
- eradication goals
	- remove incident traces
	- secure user accounts, system configs
- recovery objectives
	- restore normal operations
	- simultaneous activities with eradication 
- rebuilt systems to prevent backdoors
- media sanitization techniques: clear, purge, destroy 

### validation process
- verify the secure configuration of every system
- run the vulnerability scans
- perform account and permission reviews 

### post mortem best practices
- Do's
	- take responsibility
	- gather info
	- define clear owners
	- use a consistent template
- Dont's
	- assign blame
	- procrastinate
	- be vague
	- lose focus 

### log tampering
- why would malicious users/malwares want to tamper with logs?
- what are some processes mentioned in this article that hackers might do for log tempering?

#### log shredding in Linux
![[Pasted image 20260130101817.png]]
- could recover info from a deleted file but overwriting it multiple times makes it really hard to do

### logging laws
- laws of collection
	- do NOT collect/generate log data that you NEVER plan to use
- law of retention
	- retiain log data for as long as it is conceivable that it can be used or longer if perscribed by regulations
- law of monitoring
	- log all you can (as much as possible) but alert only on what you must respond (as little as possible)
- law of availability
	- don't pay to make your logging or monitoring system more available than your business systems
- law of security
	- don't pay to protect your log data more than you pay to protect your critical business data
- law of constant changes
	- log sources, log types, and log messages change

### backups
- how are full, differential, and incremental backup different?
	- full - backs up everything plus the new data
	- differential - saves only the data that has changed since the last full backup
	- incremental - like cloud, every change gets backed up right away
- how many backup files needed for recovery of each kind of backups?
- how would you sort different kinds of backups according to the following criteria?
	- storage capacity, recovery speed, backup time
- incidental most efficient storage capacity 
- 