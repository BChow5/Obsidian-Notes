### Proxy
- a proxy is an intermediary server that acts as a gateway between a client and destination server
- used for various purposes like improving security, enhance performance, provide anonymity
- proxy intercepts the client request to destination server and forwards the request then relays the response to client
- forward proxy or outbound proxy 
![[Pasted image 20250318090249.png]]
![[Pasted image 20250318090301.png]]

#### Benefits of proxy
![[Pasted image 20250318090715.png]]

#### disadvantages
![[Pasted image 20250318090726.png]]

#### Browser caching vs proxy server
- proxy server caching differs from browser caching
- website resources are stored in intermediate servers instead of your local drives 
![[Pasted image 20250318091524.png]]

#### Reverse Proxy
- reverse proxy sits in front of server and protects server
	- vs forward proxy sitting in front of clients 
![[Pasted image 20250318091630.png]]

#### nginx reverse proxy
![[Pasted image 20250318091859.png]]

#### HTTPS termination
![[Pasted image 20250318091916.png]]

#### Benefits and disadvantages of reverse proxy
![[Pasted image 20250318091946.png]]
![[Pasted image 20250318091953.png]]

#### Standard configuration
![[Pasted image 20250318093034.png]]

### Load balancing with HAProxy

#### What is HAProxy?
- High Availability Proxy
- open source software used for load balancing and proxying TCP and HTTP based applications
- key characteristics
	- performance - capable of high volume traffic
	- reliability - provides fault tolerance through intelligent traffic management 

#### purpose of HAProxy
- traffic distribution
- application availability 

#### key features
![[Pasted image 20250318093701.png]]
![[Pasted image 20250318093819.png]]

#### supported protocols
![[Pasted image 20250318093932.png]]

#### load balancing algorithms
![[Pasted image 20250318094102.png]]

### HAProxy architecture overview 
![[Pasted image 20250318094303.png]]
![[Pasted image 20250318094315.png]]
![[Pasted image 20250318094325.png]]

#### HAProxy configuration
![[Pasted image 20250318094443.png]]
![[Pasted image 20250318094450.png]]
![[Pasted image 20250318094505.png]]
#### Health checks
- ensure traffic is only sent to operation servers
- `server aws1 <EC2-Instance1-IP>:80 check`
- paramters
	- inter: sets the interval for checks
	- rise: number of successful checks to consider a server up
	- fall: number of failed checks for a server to be down 
	- `server aws1 <EC2-VM-IP>:80 check inter 2000 rise 3 fall 2`

#### sticky sessions
- sticky sessions ensures a user's request are routed to the same backend server based on the client's IP address or a cookie
	- crucial for applications that need session state 
![[Pasted image 20250318094713.png]]

#### monitoring
![[Pasted image 20250318095638.png]]
![[Pasted image 20250318095702.png]]
#### use cases
![[Pasted image 20250318095824.png]]

#### common commands
![[Pasted image 20250318095845.png]]
![[Pasted image 20250318095852.png]]

#### best practices
![[Pasted image 20250318095905.png]]

#### security considerations
![[Pasted image 20250318100402.png]]
