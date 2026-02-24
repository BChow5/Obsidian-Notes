### CVE (Common Vulnerabilities and Exposures)
- standardized list of publicly disclosed security vulnerabilities in software/hardware
- helps organization track, share, and prioritize security issues
- example 
	- CVE-2021-44228: Log4j2 (Log4Shell) remote code execution.  
	- CVE-2020-1472: ZeroLogon vulnerability in Microsoft Netlogon
- CWE (common weakness enumeration)
	- catalog of common software weaknesses that lead to security vulnerabilities 

### Vulnerability sources
- server
- endpoint
- supply chain
- configuration
- architectural 

#### Server Vulnerability
- weaknesses and flaws in webservers or other sever side systems
	- outdates software or unsupported OS
	- buffer overflow vulnerability
	- privilege escalation threats
	- insecure protocols (FTP, telnet, POP3)
	- server configured for debugging modes

#### endpoint vulnerabilities
- weaknesses associated with individual devices connected to a network
	- like servers but with stricter configuration
	- common issues
		- missing patches and outdated signatures
		- user reluctance to update software

#### supply chain vulnerabilities
- end of life alerts
	- e.g. monitor vendor announces for product lifecycle changes to prevent security gaps
- lifecycle stages
	- track end of sale, support, and life stages for IT product updates
- embedded and cloud risks
	- be vigilant of vulnerabilities embedded systems and the risk transfer in cloud services
- vendor oversight 
	- evaluate vendor reliability and support for cloud and data storage services

#### Configuration vulnerabilities
![[Pasted image 20260116091338.png]]

#### Architectural vulnerabilities
- incorporate security early in IT system designed to prevent fundamental flaws
- address system sprawl with propery lifecycle management of network devices
- go beyond technical aspects to include business processes and human factors
- shift left
![[Pasted image 20260116094122.png]]

### S.T.R.I.D.E
#finalQ 
![[Pasted image 20260116095607.png]]