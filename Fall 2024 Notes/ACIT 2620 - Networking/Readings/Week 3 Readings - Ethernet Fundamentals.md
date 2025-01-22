### Ethernet
-  ubiquitous and popular LAN technology
- ![[Pasted image 20240919185140.png]]

#### 10base5
- old model of ethernet 
- originally could have up to 100 units connected
- if you wanted more then you'd need to set up a bridge or repeater 
- <mark style="background: #ABF7F7A6;">CSMA/CD</mark> Carrier Sense Multiple Access with Collision Detection
	- network protocol used to manage how data is transmitted in Ethernet networks to avoid collisions 
	- particularly in older half duplex ethernet environments 
- broadcast multiple access network
	- even when sending from one device to another, you're actually sending it to everyone but they check if the destination address matches 
	- every device has their own MAC address
- half duplex (send or receive at the same time)
- forms a collision domain 
	- when two devices listen to the medium at the same time and if they send at the same time, then that is a collision 
- limited geographic distance and host count
	- limited to maybe 500m
- shortcomings led to using thig RG-58/U cable "10base2" and eventually twisted pair "10baseT" using hubs 

#### Repeater and hubs
- ethernet hubs operate at layer 1 of the OSI reference model
- ![[Pasted image 20240919190603.png]]
- ![[Pasted image 20240919190616.png]]
- when a hub gets a frame, it will repeat that frame out to all ports except for the one it came from 
- still a single collision domain
- hub responsible for detecting these collisions and sending a signal back
	- to show it had a collision then it will repeat the frame back to the port that it came from 
- least secure
	- because hubs will repeat frames out to all ports 
- hosts can receive frames not addressed to them by using promiscuous mode 
- hubs no longer used

#### Ethernet Frame
![[Pasted image 20240919191223.png]]
![[Pasted image 20240919185140.png]]
- **octets** are units of 8-bits. where byte may be ambiguous
- payload is limited to 1500 octets (MTU)
	- maximum transmission unit 
	- 1500 is usual limit of ethernet 
	- if you want to send more, you will need to break them up in smaller pieces 
		- usually the layer 3 protocol will cover this 
- **destination MAC address**
	- 6 octets - 48 bits
	- indicates where this frame is being sent to
- **Source MAC address**
	- 6 octets - 48 bits
	- where the frames are coming from
- **type of length**
	- 2 octet
	- what ether type
	- could be IP, ARP, IPv6
- **payload**
	- 46-1500 octets
- **FCS - frame check sequence**
	- checks if we had any errors with the frame 
	- the FCS is computed at the destination station
	- if the FCS matches with the original and at the destination, then we know there was no issue with the frame
- **preamble and SFD start frame delimiter**
	- preamble - 1010 pattern
	- SFD two trailing 1's
	- to sync up timing
- **interpacket **
	- 12 octet gap

#### L2 Addressing and promiscuous mode 
- interfaces are configured with a burned-in MAC address
- NICs will not interrupt the host for destination MAC addresses that do not match its interface
	- exceptions: broadcast (ff-ff-ff-ff-ff-ff) and multicast 
- you can change this behaviour with promiscuous mode, meaning the NIC interrupts the CPU for every packet 
	- packet capture software does this by default
- making a system promiscuous mode will show up in the logs 

#### MAC Addresses
![[Pasted image 20240919194609.png]]
- 48 bit hexadecimal layer 2 address
- first 24 bits are manufacturer ID
- second 24 bit is specific interface assigned by manufacturer
	- not by host
- group bit
	- indicates multicast frame
- all 1s
	- broadcast 

#### Bridges and Switches 
![[Pasted image 20240919194837.png]]
- switches create a broadcast domain from a collection of individual LAN segments 
- switches learn, filter, and forward frames based on source and destination MAC addresses
- switches can be internally partitioned using Virtual LANs (VLANs) which can be carried internally using 802.1Q tagged frames
	- Cisco calls these 'Trunks'
- some switches are also routers 
	- switching can be done at layer 3
	- 