### What are web stacks?
- software stacks enable servers to deliver web content
- LAMP and LNMP are popular webstack architectures
- focus on full-stack development

![[Pasted image 20250211084605.png]]
![[Pasted image 20250211084652.png]]

### What is LAMP?
- LAMMP stands for Linux, Apache, MySQL, and PHP
- commonly used for hosting dynamic websites
- Linux OS provides the environment for the webstack
![[Pasted image 20250211084926.png]]

### Apache: The web server in LAMP
- Apache is an open source web server
- Serves static and dynamic content
- highly configurable with various modules 
![[Pasted image 20250211084913.png]]

### MySQL: Database layer in LAMP
- relational database management system
- stores and manages application data
- widely used with web applications like WorkPress

### PHP: The scripting language in LAMP
- PHP: hypertext preprocessor
- server side scripting language for web development
- generates dynamic content 
![[Pasted image 20250211085102.png]]

### Installing Apache in Linux
- `sudo apt install apache2 -y`
- start and enable apache
- check apache status

![[Pasted image 20250211085243.png]]
![[Pasted image 20250211085253.png]]

### WorkPress overview
- WordPress is a content management system (CMS)
- widely used for blogs, businesses, websites, and e-commerce
- easy to integrate with LAMP
![[Pasted image 20250211085349.png]]

### What is LNMP?
- Linux, Nginx, MySQL, and PHP
- Nginx is a lightweight, high-performance web server
- often chosen for handling high traffic websites

### Nginx
- high performance, event driven web server
- better suited for static content compared to Apache
- can also act as a reverse proxy server

![[Pasted image 20250211090536.png]]

### Configuring PHP with Nginx
![[Pasted image 20250211090552.png]]

### Installing WordPress on LNMP
![[Pasted image 20250211090617.png]]

### Comparing WordPress on LNMP and LAMP
![[Pasted image 20250211090644.png]]
- Nginx is more efficient for static content and high traffic
- both stacks provide robust environments for dynamic websites 