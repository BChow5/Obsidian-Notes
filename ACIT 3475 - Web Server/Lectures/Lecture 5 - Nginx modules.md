### What are nginx modules?
- nginx modules are dynamic or built in extensions that enhance nginx's functionality 
- they can handle various tasks like request processing, proxying, load balancing, authentication, and logging
- two types of modules:
	- built in modules
		- included in the core nginx installation
	- dynamic modules
		- installed separately and loaded as needed
- modules allow developers to extend nginx without modifying its core code

### How do nginx modules work?
- modules are triggered during request processing phases, such as
	- **rewrite phase** (modifying URLs and headers )
	- **access phase** (authentication and authorizing)
	- **content phase** (handling requests, proxying etc.)
- configuration directives enable or disable module behaviour inside nginx.conf 
- some modules require additional installation and compilation before use
![[Pasted image 20250204092245.png]]

### Creating a custom nginx module
![[Pasted image 20250204092226.png]]

#### rewrite module
![[Pasted image 20250204092433.png]]

#### proxy module
![[Pasted image 20250204092454.png]]

#### map module
![[Pasted image 20250204092535.png]]