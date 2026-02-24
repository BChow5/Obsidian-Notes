### Intro to SSL and HTTPS
#### What are they?
- sSL (secure sockets layer) is a protocol that secures communication between a web client and server by encrypting data
- HTTPS (hyper text transfer protocol secure) combines HTTP with SSL/TLS to ensure secure data transfer

#### why is HTTPS important?
- protects sensitive information (e.g. passwords, payment details) from interception
- encrypts data to ensure confidentiality and integrity 

### The need for security
- why is web security essential?
	- HTTP sends data in plain text and can easily be interpreted
	- common attacks: eavesdropping, data tampering, and identity theft
- encrypting protects sensitive data:
	- encrypting data ensures it can't be read by unauthorized parties

### What is SSL?
- SSL Overview:
	- SSL is a cryptographic protocol that establishes secure connections between clients and servers
	- SSL was replaced by TLS due to known vulnerabilities in SSL
- SSL history:
	- developed by Netscape in 1990s
	- 2.0 and 3.0 are deprecated 

### What is HTTPS?
- HTTPS = HTTP + SSL/TLS
	- secures data transfer by encrypting it
- difference from HTTP
	- HTTP transmits data in plain text
	- HTTPS encrypts data 
	- HTTPS port 443
	- HTTP port 80

### Key components of SSL/TLS
- **Encryption**:
	- data in encrypted to prevent unauthorized access
- **authentication**
	- SSL/TLS authenticates the identity of the server, ensuring that users are communicating with the intended party
- **integrity**
	- SSL/TLS ensures data is not altered in transit 

### How SSL/TLS works
- **SSL/TLS handshake**
	- client and server exchange cryptographic keys to establish a secure session
	- handshake ensures both parties agree on encrypting protocols and share  a session key
- encrypting in action
	- SSL/TLS uses both asymmetric encryption (for key exchange) and symmetric encrypting (for data transfer)
![[Pasted image 20250304084806.png]]

### SSL handshake details
![[Pasted image 20250304084827.png]]

### SSL/TLS protocol versions
![[Pasted image 20250304084852.png]]

### types of encryption
- **symmetric encryption**
	- same keys used for both encrypt and decrypt
	- fast but requires secure key exchange
- **asymmetric encryption**
	- public key encrypts the data and a private key decrypt it
- **combined usage**
	- SSL uses asymmetric encryption for key exchange and symmetric encryption for the actual data transfer
![[Pasted image 20250304085442.png]]

### digital certificates
- what are digital certificates? 
	- electronic documents that verify the ownership of a public key by a server or website
- role in SSL/TLS
	- issues by trusted Certificate Authorities
	- they authenticate the server's identity
- certificate structure
	- contains the server's public key, information about the owner, and the CA's signature 
![[Pasted image 20250304085457.png]]

### X.509 certificate format
![[Pasted image 20250304085519.png]]

### Certificate Authority (CA)
- what is CA?
	- a trusted  third party organization that issues digital certificates
- role in SSL/TLS
	- CAs validate the identity of website owners before issuing a certificate 
	- ensuring users trust the server's identity
- examples of CAs
	- Let's Encrypt
	- Comodo
	- DigiCert 
![[Pasted image 20250304091200.png]]

### types of SSL certificates
- **single domain** 
	- protects 1 domain
- **multi domain (SAN)**
	- protects multiple domains
- **wildcard**
	- protects a domain and all of its subdomains 

### self signed certificates
- what is a self signed certificate?
	- a certificate created and signed by the server itself
		- not by a trusted CA
- use cases
	- commonly used for internal testing and development
- drawbacks
	- browsers do not trust self-signed certificates
	- leads to security warnings  

### how to obtain an SSL certificate

![[Pasted image 20250304091449.png]]

### Let's Encrypt: Free SSL certificates
![[Pasted image 20250304091509.png]]

### Certificate validation levels
![[Pasted image 20250304092124.png]]

### installing SSL on nginx
![[Pasted image 20250304092144.png]]

### installing SSL on apache
![[Pasted image 20250304092157.png]]

### HTTPS redirect
- why redirect HTTP to HTTPS?
	- ensure all traffic is securely encrypted, prevent downgrade attacks, improve search engine rankings

### SSL/TLS security best practices
- use strong cipher suites
- disable SSL and TLS 1.0/1.1 (only allow 1.2/1.3)
- enable HTTP strict transport security (HSTS) to enforce HTTPS
- use **forward secrecy** to ensure session keys are not compromised

### test your HTTPS setup
![[Pasted image 20250304092816.png]]

### common SSL/TLS issues
- expired certificates
	- need to be regularly renewed
- incomplete certificate chain
	- ensure intermediate certificates are included in the server's certificate to complete the chain of trust
- mixed content warnings
	- occurs when a webpage loads some elements in HTTP and some in HTTPS 

### SSL certificates on AWS
![[Pasted image 20250304093002.png]]

### performance considerations with SSL
![[Pasted image 20250304093743.png]]

### SSL for microservices and APIs
![[Pasted image 20250304093803.png]]

### future of SSL/TLS
![[Pasted image 20250304093824.png]]