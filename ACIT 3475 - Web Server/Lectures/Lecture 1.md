### What is a web server?
- definition
	- a web server is a computer system that hosts websites and delivers web content to users over the internet
- key functions
	- serve HTML pages and resources (images, CSS, JS)
	- handle client request and responses 
![[Pasted image 20250107085646.png]]

### Components of a web server
- components:
	- hardware (server machine)
	- operating system
	- web server software (apache, nginx, caddy)
	- configuration files
	- content (HTML, CSS, JS files)
![[Pasted image 20250107085747.png]]

### Website vs web server
- website:
	- a collection of web pages accessed via a web browser
	- static vs dynamic websites
	- e.g. personal blogs, e-commerce sites, corporate websites 
- webserver:
	- hosts websites and web applications 

### Website vs web application
![[Pasted image 20250107085930.png]]
- websites are mostly information
- web application is interactive and functional 

### Web architecture history
- evolution
	- early static HTML sites
	- introduction of server side scripting (CGI, PHP, ASP)
- milestones
	- development of HTTP. HTML. and the World Wide Web
- modern developments
	- rise of dynamic content and web applications
	- introduction of modern web frameworks and cloud hosting
- trends
	- microservices, serverless architecture, containerization 
![[Pasted image 20250107090538.png]]

### Internet vs WWW
- internet
	- a global network of interconnected computers
	- components:
	- ISPs, routers, switches, protocols (TCP/IP)
- world wide web
	- system of interlinked hypertext documents accessed via internet
- differences
	- internet is the infrastructure
	- WWW is the service running on it 

### Core components of web servers 
- hardware (server machine)
- operating system
- web server software (Apache, Nginx)
- configuration files
- content (HTML, CSS, JS files)

### Operation of web servers
- listening on ports
	- e.g. typically port 80 for HTTP and port 443 for HTTPS
- request processing
	- handing incoming requests and service responses
![[Pasted image 20250107091702.png]]

### Steps in handling client's request
- request handling process
	- client sends an HTTP requests to the server
	- server processes and the request
	- server fetches or generates the requested content
	- server sends and HTTP response back to client 

### Popular web server software
- Apache
- Nginx
- Microsoft internet information services (IIS)

### Common features of web servers
- static and dynamic content serving
	- ability to serve HTML, CSS, JS files, and dynamic content via server side scripting
- SSL/TLS Support
	- secure connection for HTTPS 

### Advances features of web servers
- virtual hosting
	- serve multiple websites on a single server
- load balancing and reverse proxy
	- distribute incoming traffic across multiple servers
- logging and monitoring
	- track and analyze server performance and usage 

### Advantages of using web servers
- reliable content delivery
	- consistent and efficient service of web content
- scalability
	- handle increasing amounts of traffic and data
- security
	- protect data through encrypting and secure protocols 

### Practical use of web servers
- usage scenarios:
	- hosting websites and web applications
	- serving APIs for mobile and web apps
	- content management systems
- e.g. 
	- real world applications 

### Authentical mechanisms in web servers
- basic and digest authentication
	- simple methods to authenticate users
- OAuth and OpenID Connect
	- Advanced protocols for secure authentication
- JWT (JSON web tokens)
	- token based authentication method 

### Factors affecting web server performance
- Hardware Resources:  
	- CPU, RAM, disk space  
- Network Bandwidth:  
	- Internet connection speed and quality  
- Configuration and Optimization:  
	- Server settings and performance tuning  
- Caching Mechanisms:  
	- Use of caches to speed up content delivery

### Optimizing web server performance
- load balancer
- content delivery networks (CDNs)
- resource compression (Gzip)
- efficient resource management 

### Path translation in web servers
![[Pasted image 20250107094408.png]]
- aliases and redirections
	- creating URL aliases and redirects
- handling dynamic routes
	- managing dynamic content and user-specific paths
- examples
	- specific scenarios for path translation 

### Understanding the website
- componenets
	- front end (client side)
	  back end (server side)
	- database
- examples
	- LAMP (Linux, Apache, MySQL, PHP)
![[Pasted image 20250107095416.png]]

### Common webstacks
- Examples:  
	- MERN (MongoDB, Express, React, Node.js)  
	- MEAN (MongoDB, Express, Angular, Node.js)  
- Detailed Explanation:  
	- How each component interacts