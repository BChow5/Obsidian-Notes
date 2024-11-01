![[Pasted image 20240920133213.png]]
- ethernet is an umbrella term
- a large collection of standards and specifications
	- like the way network cables are manufactured/designed 
- everything networking is defined and standardized by IEEE
- IANA deals with things like IP addressing and other internet protocol resources 
### Data Link Layer Sublayers 
#### Logical Link Control (LLC)
- used to facilitate multiple layer (i.e. network) protocols
- provides common interface to upper layers
- supplies multiplexing and flow control services 
- provides error checking 

#### Media Access Control (MAC)
- provides more addressing and channel access control mechanisms (i.e. CSMA, CD, CSMA CA)
- appends physical address of destination computer onto the frame 

### Frames
![[Pasted image 20240927142720.png]]
- <mark style="background: #ABF7F7A6;">preamble</mark>
	- marks the beginning of the entire frame
	- role is to synchronize the 2 devices sending and receiving 
- ethernet header
	- destination MAC address - 48 bits
	- Source Mac Address - 48 bits
	- EtherType 
		- tells you the type of the payload 
	- MAC header is 14 bytes
	- this is all metadata 
- <mark style="background: #ABF7F7A6;">start of frame delimiter (SFD)</mark>
	- indicates beginning of addressing fields
- <mark style="background: #ABF7F7A6;">destination address</mark>
	- contains destination node address
- <mark style="background: #ABF7F7A6;">source address</mark>
	- contains address of sender node
- <mark style="background: #ABF7F7A6;">length (LEN)</mark>
	- indicates length of data/payload
	- official version uses the length field 
	- minimum should be 46 bites
		- if less then frame needs to have pads
	- 
- <mark style="background: #ABF7F7A6;">data (payload)</mark>
	- contains data, or segmented part of data, transmitted from originating node 
- <mark style="background: #ABF7F7A6;">pad</mark>
	- used to increase size of the frame to its minimum size requirement of 46 bytes
- <mark style="background: #ABF7F7A6;">frame check sequence</mark>
	- provides algorithm to determine whether data was correctly received
	- most commonly used algorithm is <mark style="background: #ABF7F7A6;">Cyclic Redundancy Check (CRC)</mark>
	- part of the protocol but not part of the frame 
- cannot send anything larger than 1500 bytes 

### Ethernet Addressing
- MAC address: Media Access control (MAC) sub layer
- 48 bits
- number uniquely defining a network node
- generally rendered as hexidecimal
- doesn't contain any data regarding network location, just an ID
- ![[Pasted image 20240927150338.png]]
- first three bytes
	- either manufacturer hard coded
	- or reserved addresses (common ones)
		- broadcast address - FF:FF:FF:FF:FF:FF
		- spanning tree multicast - 01:80:c2:00:00:00
		- IANA reserves all addresses starting with 00:005E see ethernet numbers (this includes IPv4 multicast and inserts the low 23 bits of the multicast IPv4 address into the ethernet address)
		- 33:33:xx is reserved for IPv6 multicast 
- **Locally Administered Bit**: For a MAC address to be locally administered, the **second least significant bit** of the first byte should be set to `1`. This means:
	- If the first byte starts with **0x02**, the MAC address is considered locally administered. For example, `02:XX:XX:XX:XX:XX`.
### Switching
- ![[Pasted image 20240927152729.png]]
- making forwarding decisions
- transparent bridging 
	- first time it gets a new signal, it will need to send it out as a broadcast if it doesn't know where it's meant to be sent to 
	- uses this to track the MAC addresses it knows 
	- switch will need to map which MAC addresses it has 

### Broadcast Loop and STP
![[Pasted image 20240927155054.png]]
- if for some reason you had a duplicate in the addresses, then the data will be sent through both ports 
- could accidentally introduce a loop 
- looping traffic is bad for a network
	- will break it down due to so much traffic
- you want to introduce redundancy without creating looping traffic 
- STP is the solution to avoid accidentally looping traffic when trying to create redundancy 

#### STP Spanning Tree Protocol
![[Pasted image 20240927155140.png]]
- for redundancy 
- switches will communicate with themselves
- they decide and disable one of the links
	- disables logically. not physically 
- it still has redundancy because if a link goes down, they can reenable the disabled link to use instead
	- restores the full connectivity 

### VLANs
- Virtual Local Area Networks
- a logical network within a physical network
- achieved by grouping some of the switch ethernet ports into a logical broadcast domain
- can span multiple switches 
- we assigned VLAN numbers to certain ports on the switch 
- ![[Pasted image 20240927155949.png]]
	- physically one network but logically split into 3

#### VLAN port types
- access ports
	- assigned VLAN ID
	- for connecting end hosts/nodes
	- nodes connected to ports with same VLAN ID are in the same broadcast domain 
- Trunk Ports
	- typically for switch to switch or  switch to router connection
	- carry "tagged" frames
		- i.e. modified ethernet frames with VLAN markers 

#### Tagged Frames
![[Pasted image 20240927160224.png]]
- 4-byte tag header inserted between Source MAC and EtherType fields
	- 2byte tag protocol identifier (TPID)
		- a fixed value of 0x8100 that indicates the frame carries tag info
- 2-byte tag control information (TCI)
	- Three-bit user priority (used to prioritize traffic
	- Drop Eligible Indicator (DEI) (in congestion is frame “droppable”
	- Twelve-bit VLAN identifier (VID)-Uniquely identifies the VLAN to which the frame belongs

### Link Access Methods
- managed shared medium access contention (collision)
- two methods:
	- CSMA/CD - for wired ethernet
		- cs = carrier sense (listen)
		- ma = multiple access 
		- in a shared medium network, you have to listen before you send things out 
		- collision detection 
	- CSMA/CA - for wireless ethernet 
		- collision avoidance

#### CSMA/CD
- ![[Pasted image 20240927160525.png]]
	- if there is a collision, back off and try again after 
	- can always detect when someone is transmitting because it is physically connected 

#### CSMA/CA
![[Pasted image 20240927160728.png]]
- can't always tell when someone else is transmitting
- instead sends a signal to the controller (your Wi-Fi) to get a clear to transmit from the controller 
- RTS = request to send
- CTS = clear to send
- ack = acknowledgement 