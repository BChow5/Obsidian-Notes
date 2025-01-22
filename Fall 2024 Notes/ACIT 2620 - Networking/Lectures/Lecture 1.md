 ### What is the internet?
- what is it made of?
- what is its purpose?
- how does it work?

### Network
- Collection of <mark style="background: #ABF7F7A6;">nodes</mark> connected by some type of transmission media or <mark style="background: #ABF7F7A6;">link</mark>, for the purpose of sharing services, devices or data (i.e. networked resources)

##### Node
- Any device that can communicate over the network and is identified by a unique identifying number, known as it's <mark style="background: #ABF7F7A6;">network address</mark> 
- e.g. routers, servers, client devices 

##### Link
- anything that carries signal from one node to another node
- e.g. cables, switches, modem, hubs
![[Pasted image 20240906142357.png]]

### Media Concurrency and Direction

![[Pasted image 20240906145601.png]]
- half duplex
	- 2 directly but only 1 at a time
- full duplex
	- both directions can be at the same time
- simplex 
	- only goes in 1 direction 

### Resource Control
- e.g. website 

### Client-Server Networks

![[Pasted image 20240906150145.png]]

### Peer to Peer Networks

![[Pasted image 20240906150208.png]]

### Types of Networks
- LAN
- WLAN
- PAN
- CAN
- MAN
- WAN
- SAN
- EPN
- VPN

### Switching Method
- Switching is the act of delivering a message within a network

#### Circuit Switching
- physical switching that was norm in telephone technology
- problem:
	- needs to reserve a path of communication and it becomes difficult when it has to go further
	- leaves a lot of the network under utilized 

![[Pasted image 20240906151732.png]]

#### Packet Switching
- Internet uses packet switching
- all you need to do is break down your message into smaller pieces (packets) and send them over
- maximizes link utilization 
- because they are in smaller pieces, they don't need to take the same path 
- different packets can be sent concurrently
	- it will be reassembled at it's destination 

![[Pasted image 20240906151746.png]]

### Layered Networking Model

![[Pasted image 20240906153248.png]]
- **Application Layer**: Provides protocols for end-user services like HTTP, SMTP, and FTP, facilitating communication between software applications.
- **Transport Layer**: Manages end-to-end communication, ensuring reliable data transmission through protocols like TCP (connection-oriented) and UDP (connectionless).
- **Network Layer**: Provides connectivity between end hosts on different networks (e.g. outside of the LAN). Provides logical addressing (IP addresses). Provides path selection between source and destination 
- **Link Layer (Data and Physical)**: Responsible for physical transmission of data between devices on the same local network and includes protocols for managing data frames, error detection, and hardware addressing (e.g., Ethernet).
#### Why Layers?
- Managing complexity
	- explicit structure allows identification and makes explicit the relationship of complex system's pieces
- Modularization
	- changing of an implementation of a specific layer's service is hidden from the rest of the system 

### Protocol Data Units (PDU)

![[Pasted image 20240906161001.png]]
- meta data is put into headers to show destinations of packets 

### Protocols

![[Pasted image 20240906161103.png]]
- mutually agreed upon rules for communication
- define the format, order of messages sent and received among network entities, and actions taken upon transmission, receipt, and timeout
- govern all communication activity on the internet 

