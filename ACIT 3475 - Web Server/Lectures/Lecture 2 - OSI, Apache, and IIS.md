### OSI Model
- OSI = Open System Interconnection
- standard for systems open to communication with other systems
- model has 7 layers 
![[Pasted image 20250114083854.png]]
![[Pasted image 20250114083913.png]]
![[Pasted image 20250114083936.png]]

#### Internet Protocols vs OSI
![[Pasted image 20250114083958.png]]

### Apache HTTP Server (web server)
- open source and free web server
- powers around 31% of websites
- allows website owner to serve content on the web

#### Features
- implements many frequently requested features including:
	- DBM databases for authentication
	- customized response to errors and problems
	- virtual hosts
	- multiple directory index directives
	- has been thoroughly tested

#### Pros
- open source and free. even for commercial use
- realiable and safe
- frequently updated, regular security patches
- flexible due to its modular base structure
- easy to configure
	- beginner friendly
- cross platform
- works out of the box with WordPress sites 
- huge community and easily available support 

#### Cons
- performance problems of extremely heavy traffic  sites
- too many configuration options can lead to security vulnerabilities 

### Apache Basic Configuration
- to configure Apache, you need to edit the configuration files, which are usually located in the<APACHE_HOME>/confdirectory. The main configuration file is called `httpd.conforapache2.conf`, depending on your system.  
- You can also include other configuration files using the Include directive:  
	- **Listen**: This directive specifies the port number and the IP address that Apache will listen to for incoming requests. E.g., Listen 80  
	- **ServerName**: This directive sets the hostname and the port that Apache will use to identify itself.E.g.,www.adatum.com:80  
	- **DocumentRoot**: This directive sets the directory where Apache will look for the files to serve. E.g., DocumentRoot/var/www/html You can use this directive to control the access, authentication, and caching of your directories (/var/www/html and its subdirectories).  
	- **Directory**: This directive allows you to set the permissions and options for a specific directory or a group of directories. E.g., <Directory /var/www/html>  
	- **VirtualHost:** This directive allows you to create virtual hosts, which are separate configurations for different websites or domains

### What is IIS
- Internet Information Services
- web server software created by Microsoft
	- paid software
- hosts ASP.net applications, static websites
- a component of Windows OS
- offers extensibility using modular architecture and API
- supports HTTP/2, caching, and HTTP/s
- deep integreation with ASP.net
	- framework for dynamic applications 

#### What are the requirements
- hardware
	- CPU - 1ghz or faster
	- RAM - 512 MB - 2GB
	- storage - 10GB or more
- software
	- .NET framework
	- windows OS

#### Why use IIS?
- integration with Windows
- security
	- built in request filtering and supports other security features like SSL
- scalability
	- IIS can handle large scale operations, making it good for both small and large businesses
- manageability
	- offers GUI for server management

### Comparison
![[Pasted image 20250114091334.png]]
![[Pasted image 20250114091346.png]]

### Why use HTTPS?
- security
	- secure commuinication between client and server
- data integrity
	- certificate includes server public key (ADCS)
- user trust 

### Drawback of IIS
- platform dependency
	- tied to windows ecosystem
	- less versatile
- licensing cost
- performance
	- while highly efficient, certain benchmarks and use cases might favour the performance of servers like Nginx or Apache
- customization and modules
	- although IIS is extensible, the open source counterparts often have a large pool of modules and extensions available 