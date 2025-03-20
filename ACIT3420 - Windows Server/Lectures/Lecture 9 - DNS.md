### DNS Hierarchy
- #finalQ know which are the top level ones
	- know what second level is 
- com, edu, gov. au
- 13 root servers across the world 

### Types of DNS Zones
- #finalQ know the difference and when to use 
- **primary**
	- read/write copy of a DNS database (authority DNS)
		- stored in a text file
	- easy to back up and recover
	- must be available to make changes
- **secondary**
	- read only copy of a DNS database
		- can be copies of primary, secondary, and active directory integrated 
	- primary zone must be available to make changes
	- windows can be a secondary zone to a Unix primary zone
	- can improve performance through load balancing
	- for security 
- **stub**
	- copy of a zone that contains only records used to locate server names
		- redirects requests to a server that can answer it
	- subset of records
		- Glue Host (A) records, Start of Authority (SOA), Name Server (NS) records 
		- start authority
			- complete copy of the domain's zone file (DNS) 
	- using stub zone as a forwarder is better because you don't need to update since it's synced with NS records 
		- forwarder you would need to update manually for the ip changes
- **Active Directory Integrated**
	- zone data is stored in AD DS rather than zone files
	- only available on domain controllers
	- high availability and redundancy
	- harder to restore in a disaster 



### DNS Records
- #finalQ 

### Forwarders and conditional forwarders
- #finalQ 
- when do you need a conditional forwarder?
	- when you want to make a shortcut instead of searching all of the domain/root 
	- if you want to get somewhere that isn't using a qualified domain 
		- e.g. abcd.history
		- any domain that can't be registered. private domain 
		- domain name can't be resolved in the top or second level 

### DNS Security 
- #finalQ 
- DNSSEC
- DNS Socket Pool
- DNS cache locking 