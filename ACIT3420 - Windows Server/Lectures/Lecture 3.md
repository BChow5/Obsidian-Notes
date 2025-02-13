### domain service (ADDS)
- server providing **authentication** and **authorization**
	- authentication - 
	- authorization - 
- has a **database** of accounts in it
- if A and D are in the server, A can get files from D using the server 
- **NTDS.dit** (database)
- ADDS
	- domain service
	- stores your objects (users, computers, groups, printer)
- what is domain controller?
	- #midtermQ

### physical component and logical component of ADDS
- #midtermQ 
- physical is not about the hardware for this

#### Physical
- #midtermQ
- <mark style="background: #ABF7F7A6;">database (NTDS.dit)</mark>
	- #midtermQ
	- 4 partitions
		- **domain**
			- includes objects (users, computers, groups, printer)
		- **schema** 
			- class
				- blueprint, category 
			- attribute 
				- used to describe the class
		- **application**
			- third parties 
				- e.g. DNS records (A, AAAA, SRV, MX, NS)
					- A is ipv4, AAAA is ipv6, MX is mail, SRV is who is domain controller 
		- **configuration** 
			- network infrastructure 
			- domain architecture/structure 
- <mark style="background: #ABF7F7A6;">network authentication protocol </mark>
	- need encryption 
	- **Kerbos** is network authentication protocol 
- <mark style="background: #ABF7F7A6;">DNS</mark>
	- SRV record = who is domain controller
		- SRV is service location 
	- forward lookup zone
		- full qualified domain name to IP lookup
	- reverse lookup zone
		- IP to name lookup 
	- **DNS port number 53**
	- support TCP and UDP
	- **remote desktop port number 3389** 
	- **mySQL port number 3306** 
	- **Microsoft sql server port number 1433** 
- <mark style="background: #ABF7F7A6;">LDAP</mark>
	- communication protocol
	- NTDS.dit follows this to store our data 
	- for queries 

#### Logical 
- **forest**
	- more than 1 tree
- **tree**
	- more than 1 node or domain 
	- your first domain controller is the root of the forest and the root of the tree 
- **site**
	- group of domain controllers 
	- for replication purpose 
- **ou**
	- organizational unit
	- how many you need is based on your company architecture
		- e.g. how many departments you have 
- binary tree