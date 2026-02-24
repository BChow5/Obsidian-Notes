![[Pasted image 20250128084141.png]]![[Pasted image 20250128084152.png]]

### What is Nginx?
- open source web and reverse proxy server
- high performance HTTP, HTTPS, SMTP, IMAP, POP3 server
- load balancing and HTTP caching
- asynchronous event driven architecture 
	- does not need to wait 

### Why use Nginx?
- lightweight with small memory footprint
- uses predictable memory under load
- provides high level of concurrency
- serves static content quickly
- handles connections asynchronously
- uses single thread

### Nginx important features 
- event loop
- non blocking i/o
- worker process 
	- each worker process takes 1 thread of a CPU core 
	- takes care of all the incoming connections 
	- will sit on the thread and processes all connections to it. not just 1
- efficient resource utilization
	- uses shared memory part of the RAM

#### installing nginx
- `sudo apt install nginx`

#### starting/restarting nginx
- check that it is running
	- `sudo service nginx status`
- starting, stopping, restarting
	- `sudo service nginx start`
	- `sudo service nginx stop`
	- `sudo service nginx restart`

### nginx process model
![[Pasted image 20250128085258.png]]

#### Master process
- reads and validates configurations
- creates and binds sockets
- creates child processes
- cache loader -> loads cache in memory
- cache manager -> prunes cache periodically 
- worker process -> handles connections, IO, and communicates to upstream server 

#### Worker process
![[Pasted image 20250128090159.png]]
- single threaded
- one worker process per CPU core
	- directives say how it behaves 
	- e.g. `worker_processes auto;`
- communicates with each other using shared memory
- no hierarchy 
- handles multiple connections asynchronously
- polls for events on listen and connection sockets
	- reacts to new changes without blocking other operations 
- events on listen sockets start new connection
- events on connection socket handles subsequent requests
- connections are submitted to state machine
	- HTTP
	- Stream
	- Mail (SMTP, IMAP, and POP3)

### High availability
![[Pasted image 20250128090519.png]]
- as simple as doing
	- `nginx -s reload`
	- `-s` sends signal to master process
- small CPU spike


### Nginx config
![[Pasted image 20250128090608.png]]

### Server config
![[Pasted image 20250128090627.png]]