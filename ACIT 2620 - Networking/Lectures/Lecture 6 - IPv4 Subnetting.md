### Network Layer
- Goal: move packets for source to destination
	- Path Determination:
		- the calculation of the route taken by packets -> routing
	- Forwarding:
		- The movement of a packet from one network to the next appropriate network

#### Network Layer Functions
![[Pasted image 20241011141544.png]]
- routers do most of the work

### Internet Protocol (IP)
- provides info about how and where data should be delivered
- responsible for internetworking (from where the term internet is derived)
- to internetwork is to traverse more than one LAN segment and more than one type of network through a router
- in an internetwork, the individual networks that are joined together are called subnetworks
- IP is unreliabe, connectionless protocol.
	- this means it does not guarantee deliver of data
	- just shows the way for the data
- e.g. IP will service a request without requesting verified session and without guaranteeing deliver of data
- makes it simple and fast 

### IP Addressing
- **IP address** - 32 bit identifier for host, router, interface 
	- size = 2^32 for size of IP address
- **interface** - connection between host, router, and physical list
	- routers typically have multiple interfaces
	- hosts may have multiple interfaces
	- IP addresses associated with interface, not host, router 
![[Pasted image 20241011142143.png]]

#### Components of an IP address
- network part (high order bits)
- host part (low order bits)

#### IP Network
- device interfaces with same network part of IP address
- can physically reach each other without intervening router
![[Pasted image 20241011142733.png]]

### IP Address Space
![[Pasted image 20241011143411.png]]
- class a: **x**.x.x.x
	- 0.....
	- just look at the first bit
	- 2^24 options for host network
- class b: **x.x**.x.x
	- 10.....
	- look at first 2 bits
	- 2^16 options for host network
- class c: **x.x.x**.x
	- 110.....
	- look at first 3 bits 
	- 2^8 options for host network
- the bold ones are the network address 

### IP range notations
- CIDR - classless inter domain routing
	- network portion of address of arbitrary length
	- address format a.b.c.d/x 
		- where x is the # of bits in network portion of address
	- also written as address + subnet mask
- ![[Pasted image 20241011150637.png]]
- ![[Pasted image 20241011151637.png]]
	- CIDR using subnet mask

### Special addresses
- first address in the range is the network ID
- last address in the range is the broadcast ID
- so these 2 addresses are always reserved for that and can't be assigned 
- always include the subnet prefix length when you write out an IP address
- Private IP Addresses
	- 10.0.0.0 -> 10.255.255.255
	- 172.16.0.0 -> 172.31.255.255
	- 192.168.0.0 -> 192.168.255.255
- Documentation IP Addresses
	- 192.0.2.0 -> 192.0.2.255
- Self-Configured IP Addresses (often DHCP Failure)
	- 169.254.0.0 -> 169.254.255.255
- Unknown Address
	- 0.0.0.0
- Loopback Address
	- 127.0.0.1 (actually, 127.0.0.0/8)
	- the entire range starting with 127
- Network Address (All host bits set to 1)
	- E.g: 192.168.1.0, Subnet Mask = 255.255.255.0

### Broadcasting and Multicasting
- limited broadcast
	- 255.255.255.255
	- transmitted only on local segment - not routed
	- only works within a LAN
- network broadcast address
	- network address + all host bits set to one
	- e.g. network address = 192.168.1.x
		- network broadcast address = 192.168.1.255
	- works across networks (sending through routers)
- Multicast Address
	- Lie within the 224.0.0.0 /4 network
	- any address starting from 224-239 is a multicast address
	- multicast does not care about LAN or network 
	- http://www.iana.org/assignments/multicast-addresses

### IPv4 header
![[Pasted image 20241011155445.png]]
- version - number 4
- IHL - internet header length
	- specifies the length of the ipv4 header
- DS field
	- used to prioritize network traffic
	- Different DSCP values correspond to different service levels or traffic classes, enabling routers to handle packets according to their priority
- ECN field - explicit congestion notification
	- router lets the sender know it's experiencing congestion
- identification
	- uniquely identifies each packet sent from a source to a destination
	- used for reassembly of fragmented packets
	- so you know what order to reassemble them 
- flags
	- 3 bits field used to control and manage the fragmentation of packets
	- bit 0 = reserved
	- bit 1 = don't fragment
		- 0 = fragmentation is allowed
		- 1 = don't fragment the packet
	- bit 2 = more fragments
		- 0 = this is the last fragment
		- 1 = more fragments on the way 
- fragment offset
	- used when a packet is fragmented, allowing the receiver to reassemble the fragments in the correct order.
	- When a large packet is fragmented, each fragment may carry only a portion of the data. The **Fragment Offset** helps the receiving device place these fragments in the correct sequence to reassemble the original packet.
- protocol
	- indicates the content of the packet
	- TCP or UDP or ICMP

### IPv4 Routing

![[Pasted image 20241011161300.png]]