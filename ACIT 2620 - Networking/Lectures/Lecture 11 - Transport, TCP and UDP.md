## Transport Layer Protocols 

### Transport Services and Protocols
- provide logical communication between app’ processes running on different hosts  
- transport protocols run in end systems  
- network layer: data transfer between end systems  
- transport layer: data transfer between processes  
	- relies on, enhances, network  layer services
![[Pasted image 20241122140036.png]]

### Multiplexing / demultiplexing 
- segment
	- unit of data exchanged between transport layer entities 
- demultiplexing
	- delivering received segments to correct app layer processes 
![[Pasted image 20241122140247.png]]
- segments can just be sent out of order because each segment has its unique indicator to let the host identify which goes where and what order 
- multiplexing
	- gathers data from multiple app processes, enveloping data with header (layer used for demultiplexing)
- based on sender, receiver port numbers, IP addresses
	- source, destination port numbers in each segment
	- well known port numbers for specific applications 
![[Pasted image 20241122140611.png]]

### Socket
- logical address assigned to a specific process running on a host computer
- the socket's address combines the host computer's number with the port number associated with a process

### Port numbers
![[Pasted image 20241122141301.png]]

### UDP User Datagram Protocol
- barebones internet transport protocol
- no guarantee of delivery
- no connection needed
- UDP segments may be:
	- lost
	- delivered out of order to app
- connectionless
	- no handshaking between UDP sender, receiver
	- each UDP segment handled independently of each other 
- why is there UDP?
	- no connection establishment (which can add delay)
	- simple
		- no connection state at sender and receiver 
	- small segment header
	- no congestion control
		- UDP can blast away as fast as desired 

#### UDP uses and format
- often used for streaming multimedia apps
	- loss tolerant
	- rate sensitive
- other UDP uses
	- DNS
	- SNMP
- reliable transfer over UDP
	- add reliability at application layer 
![[Pasted image 20241122142405.png]]

### TCP Overview
- point to point
	- one sender, one receiver
- reliable, in order byte stream
	- no "message boundaries"
- pipelined
	- TCP congestion and flow control set window size
- send and receive buffers
- full duplex data
	- bidirectional data flow in same connection
	- MSS - maximum segment size
- connection oriented
	- handshaking unit's sender, receiver state before data exchange
- flow controlled
	- sender will not overwhelm receiver 

### TCP Segment Structure
![[Pasted image 20241122145648.png]]

#### TCP sequence numbers and acknowledgements
![[Pasted image 20241122145911.png]]

### Net filter and Packet filtering 
