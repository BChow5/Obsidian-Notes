- stride
	- know what it is
	- know the table
	- 2 questions
- different kinds of hackers and their intentions
- different hackers in terms of skillset
	- what do we call them?
- short situation and need to answer the type of attack, which aspect of CIA triad is being compromised
	- be able to distinguish which part of CIA is being compromised
- cve, cwe, cvss, nvd, CM system
	- know what all these are and what they stand for
- encryption algorithms
	- RSA, AES,
	- syemmtric or asymmetric
- hashing algorithms
	- need to know which are hashing algorithms and not encryption ones
- vulnerability sources
	- 2 questions
	- examples of them 
- social engineering
	- decide which social engineering factor is the scenario about
- malware
	- different types
- facts about bot nets
	- different nodes in the system
	- how is the connection
- strategy for backing up data
	- indicators of back up time, speed storage
	- which are fastest, etc.
	- differential is always the middle
	- know the pros and cons
- encryption keys
	- know which encryption algorithm has 1 key
	- know which has 2
	- public key to encrypt
	- private key of same pair to decrypt
- digital signature
	- How making it works
	- verification of them
- revoking digital certificate
	- how that happens
	- 1 question
- what is self signed certificate
	- server and CA certificate

## Week 1
### CIA Triad
- confidentiality
	- ensures that info is accessible only to authorized individuals
	- encryption, passwords, access controls, MFA
- integrity
	- ensures info remains accurate, complete, and unaltered unless changed by authorized users
	- uses hashing, checksums, digital signature, version control
- availability
	- ensures info and system are accessible when needed by authorized users
	- uses back ups, redundancy, failovers


| Security Control Example                      | Objectivity     |
| --------------------------------------------- | --------------- |
| Access Control                                | Confidentiality |
| Version Control                               | integrity       |
| Encryption                                    | confidentiality |
| Ensuring fault tolerance                      | availability    |
| Dedicating multiple servers to one operations | availability    |

## Week 2

### CVE Common Vulnerabilities  and Exposures
- **CVE** - standardized list of publicly disclosed security vulnerabilities in software/hardware
- helps oprganizations track, share, prioritize security issues 
- **CWE** - common weakness enumeration
	- catalog of common software weaknesses that lead to security vulnerabilities

### vulnerabilities sources
- server 
	- weakness or flaw in web server or other ser side systems
	- outsated software/unsupported OS
	- buffer overflow
	- insecure protocols
- end point
	- weakness associated with individual devices connected to a network
	- missing patches and outdated signatures
	- user reluctance to update software
- supply chain
	- end of life alert
	- life cycle stages
	- embedded and cloud risks
	- vendor oversight 
- configuration
	- avoid default configurations
	- verify device security before network integration
	- follow documented security standard and best practices
	- choose secure cryptographic protocols and strong ciphers
	- apply the principle of least privilege 
- architectural
	- incorporate security early in the IT system design to prevent fundamental flaws
	- go beyond technical aspects to include business processes and human factors

### S.T.R.I.D.E

| Threat                 | Property        | Definition                                     |
| ---------------------- | --------------- | ---------------------------------------------- |
| Spoofing               | authentication  | impersonating something or someone             |
| Tampering              | integiry        | modifying data or code                         |
| Repudiation            | non-repudation  | claiming to have not performed an action       |
| Information Disclosure | confidentiality | exposing info to someone not authorized        |
| Denial of Service      | availability    | deny or degrade service to users               |
| Elevation of privilege | authorization   | gain capabilities without proper authorization |

### standards
- Security Content Automation Protocol (SCAP)
	- framework of standards developed to automate security management, assessment, and compliance across IT systems 
- CVE and NVD databases
	- National Vulnerability Database
		- us gov database
	- Common Vulnerabilities and Exposures
- Common Vulnerability Scoring System CVSS

### vulnerability management
#### Typical Order of Stages
1. **Discover** 🔍
2. Assess / Analyze
3. Prioritize
4. Remediate
5. Validate

## Week 3

### malware propagation
- virus - spreads by attaching to legit programs
- **worm** - self replicates and spreads independently 
- **trojan** -disguises itself as a legit program
	- Remote Access Trojan RAT

### malware payloads
- **spyware** - gatherers user info
- **adware** - displays unwanted ads
- **ransomware** - encrypts user data for ransom

### logic bombs
- logic bombs set up in a computer system
- sit idle until predefine event occurs
	- time trigger
	- manual trigger
	- event trigger
- malware carried by bomb infects the system

### advanced malware 
- **polymorphic viruses** change to avoid signature detection
	- different encryption key for each system
- **armored viruses** prevent reverse engineering
	- obfuscated assembly language
	- block the use of system debuggers
	- preventing use of sandboxing 

### rootkits
- grant root access + software techniques designed to hide other software
- rootkit payloads
	- backdoors, botnet agents, spyware
- user mode vs kernel mode

### botnets
- collection of zombie computers used for malicious purposes
- steal computing power or storage/network connectivity
- indirect and high redundant command and control channels
- bots connected to Command and Control Server C&C
- all controlled by bot master
- motives
	- ransom
	- DDoS attacks
	- data theft
	- proxying/hiding identity

### malware prevention
- signature detection - suspicious activity
- behaviour detection - deviation from normal activity
- endpoint detection and response (EDR) 
	- installed agents watch for signs of malicious activity
	- auto response triggers
	- sandboxing executables

### attack vectors and attack surface
- vector - path to obtain initial access
- surface - entire area susceptible to hacking 
- examples
	- email - phishing, links
	- social media
	- removable media
	- magnetic stripe cards
	- cloud services
	- physical access
	- IT supply chain
	- wireless networks

### social engineering
- reasons for success
	- authority
	- intimidation
	- consensus
	- scarcity
	- urgency
	- familiarity

### impersonation attacks
- spam - unsolicited emails
	- phishing - elicit sensitive info
	- pharming - using fake websites
	- spear phishing - highly targeted attacks
- prepending - add fake safe tags
- credential harvesting - use compromised credentials 

### zero day vulnerability
- vulnerability that is discovered but not patched yet
- hard to exploit because they first need to be discovered
- so many because software is extreme complex

## Week 4

### incident identification
- security events
	- then adverse security events
		- then security incidents

#### definitions
- security event
	- any observable occurrence within a system or network
	- doesn't have to be harmful 
- adverse security events
	- events that negatively affect the security of system or data
- security incident
	- confirmed event or series of adverse events that result in a breach of security policy
	- e.g. data theft or unauthorized access

### SIEM Security Information and Event Management
- solution that helps organization detect, analyze, and respond to security threats 
- central log management
	- all system logs send directly to SIEM
- ai driven threat detection

### syslog
![[Pasted image 20260220193105.png]]

### severity levels
- 1 is highest and 5 is lowest
- sev 5 - low leve deficiency causing minor problems
- sev 4 - minor problem that affects the service but no serious impact on users
- sev 3 - incident that causes errors, minor problems for users, or heavy system load
- sev 2 - significant problem affecting a limited number of users in production
- sev 1 - critical incident that affects a large number of users in production 

### data sensitivity focus
- PII Personally Identifiable Information
- PHI Personal Health Information
- SPI Sensitive Personal Information
- PCI Payment Card Industry Data
- severity assessment criteria
	- downtime, recovery time, data integrity breach, economic impact 

### NIST incident response
- process for handling cybersecurity incidents
- perperation
- detection and analysis
- containment, eradication, and discovery
- post incident activity 

### containment techniques
- segmentation - dividing networks into logical segments
- isolation - move compromised systems to a network completely disconnected from the main network
- removal - completely disconnect impact systems from any network to cut off attacker access

### incident eradication and recovery
- eradication goals
	- remove incident traces
	- secure user accounts and system config 
- recovery objectives
	- restore normal operations
- rebuild system to prevent backdoors
- media sanitization techniques - clear. purge, destroy

### validation process
- verify the secure config of every system
- run the vulnerability scans
- perform account and permission reviews
- verify that systems are logging and communication to the SIEM

### post mortem best practice
- do's
	- take responsibility
	- gather info
	- define clear owners
	- use consistent template
- dont's 
	- assign blame
	- procrastinate
	- lose focus
	- be vague

### loggings laws
- law of collection
	- don't collect log data you never plan to use
- law of retention
	- retain log data for as long as conceivable that it can be used
- law of monitoring
	- log all you can but alert only on what you must respond
- law of availabilirt
	- dont pay to make your logging or monitpring system more available than you business system
- law of secirty
	- dont pay to protect your log data more than critical business data
- log of constant change
	- log sources, log type, and log messages change

### back ups
- full backup
	- copies all data every time
	- simple but time consuming
- differential backup
	- copies all changes since last full backup
	- each differential grows larger until next full backup
	- middle ground between full and incremental
- incremental backup
	- copies only changes since the last backup
	- each backup is typically small
	- fastest and most storage efficient for daily backup

## Week 5

### cryptography
- practice and study of techniques for secure communication in the presence of third parties
- objectives
	- confidentiality - no unauthorized access
	- integrity - no unauthorized change
	- authentication - proof of identity
	- obfuscation - hide sensitive data
	- non repudiation - verify origin

### encryption and decryption
- the process of encoding information so that it's not readable by unauthorized individuals

### code vs ciphers
- code - substitution of one word or phrase for another
- cipher - use mathematical algorithms to encrypt and decrypt messages
	- cipher processing techniques
		- stream cipher
			- encrypts data one bit or byte at a time
		- block cipher
			- encrypts data in fixed size blocks

### symmetric and asymmetric cryptography
- symmetric
	- uses one shared key for both encryption and decryption
	- very fast
	- not scalable
	- if key leaks, all data in compromised
	- less secure
	- e.g. AES, DES, RC3, 3DES
- asymmetric (public key)
	- public key for encryption
	- private key for decryption
	- e.g. RSA, ECC, DSA, DH
	- scales well
	- slower
	- enables digital signatures
	- provides confidentiality, non repudiation, and integrity

#### data encryption standard DES
- symmetric encryption algorithm
- considered insecure
- 56 bit key length

#### advances encryption standard AES
- symmetric encryption
- 128 bitlocker cipher
- considered secure

#### rivest, shamir, adelman RSA
- asymmetric encryption algorithm
- variable key length
- secure
- select 2 large primate numbers to create private and public keys

### hash functions
- generating a unique fixed size number (hash) from a message of an arbitrary length
- cryptography priorities
	- one way function - way way to find message given hash
	- collision resistance - find two different inputs with same hash value
- Secure hash algorithm SHA
- one way hash function applications
	- password verification
- HMAC
	- message authentication code
		- short piece of info attached to the message
		- provides authentication and integration
	- hash based MAC
		- combine the message and secret key then apply the hash function on the result

### digital signatures
- mathematical scheme for verifying the authenticity of digital messages or documents
- hashing + asymmetric key algorithm
- provides
	- authentication
	- integrity
	- non repudiation

## Week 6

### keys needed in a network
- symmetric
	- total keys = n(n-1)/2
- asymmetric
	- total keys = 2n

### digital signature verification
- digital signatures provide authenticity, integrity, and non repudiation
- signer's private key used for creating the signature
- signer's public key used for verifying the signature

### MITM attack - public key encryption
- man in the middle attack
- attacker secretly replaces the real public key with their own

### public key infrastructure
- trusted certificate authorities CA
	- verifies identity of users and their public key
	- issues digital signed digital x.509 certificate

### identification
- identity
- authentication
- authorization
- accounting
	- what did you actually do

### authentication factors
- knowledge
	- something you know
	- e.g. password, security question, pin
- possession
	- something you have
	- e.g. mobile phone, smart card, hardware token
- inherence
	- something you are
	- e.g. finger print, retina pattern, face recognition

### password attacks
- brute force
	- try all character combos until password found
- dictionary attack
	- prioritize words in dictionary over random words 
- password spraying
	- dictionary attacks that employ frequent passwords
- credential studding
	- reuse stolen credentials across different services
- rainbow table attack
	- use precomputed hash tables to crack password hacks

