### Quiz stuff
- primary vs secondary vs stub zone
	- primary DNS zone
		- read/write copy of a DNS database (Authority DNS)
			- stored in a text file
		- easy to back up and recover
		- primary zone must be available to make changes 
	- secondary DNS zone
		- read only copy of DNS database
			- can be copies of primary, secondary, and Active Directory Integrated
		- primary zone must be available to make changes 
		- Windows can be a secondary zone to a unix primary zone 
	- stub
		- copy of a zone that contains only records used to locate name servers (redirects the requests to a server that can answer it)
		- subset of records: glue host (A) records, Start of Authority (SOA), Name Server (NS) records 
- top level domain
	- a Top Level Domain is the last part of a domain name in a web address after the .
	- highest level in the hierarchal domain 
- second level domain
	- part of the domain name that is on the left of the .
	- e.g. like google
- forwarder and condition of a forwarder
	- a DNS forwarder is a DNS server that forwards DNS queries to external DNS servers 
		- can also be sent to a conditional forwarder bsaed on the domain name queried 
	- local DNS server must be configured with the ip address of the forwarder to send queries it can't resolve
	- can configure to be round robin, netmaks ordering, and secure chache against pollution option 
		- by default, these are enabled 
- how to protect your dns service 
	- DNS Security Extentions (DNSSEC), cache locking, DNS Socket pool, etc. 