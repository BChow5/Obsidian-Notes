#### 10base5
- original network setup - 10base5 lan uses coaxial between two terminators - transceiver attaches at point along the coaxial cable through a vampire tap, then goes through AUI (net attachment user interface) cable that connects to the host through the NIC - network interface card
- Carrier sensed multiple access with collision detection - CSMA/CD
	- hosts receive all information transferred along the line, if the destination address matches the address of the device, takes the information (interrupts cpu on the host) and copies info to the host
	- broadcast multiple access - for all devices
	- half duplex - send receive at the same time
	- limited to 500m and limited hosts

#### hubs (layer 1)
- operate at layer 1 (physical of OSI reference model)
- when frame comes in, it repeats the signal to all other ports except the one that it came from - no filter or modifying
- collision domain - hub is responsible for signal management
- if collision, repeats signal back to the original port
- the least secure - repeats frames to all ports - in promiscuous mode it can accept frames not addressed to itself (host)

#### Broadcast Domain vs Collision Domain
A broadcast domain is a group of nodes sharing the same transmission medium (i.e LAN or simply network). When these nodes are not connected through a switch (or bridge), the network constitutes a single collision domain.

#### frames (layer 2)
- the information sent along the ethernet
- info is measured in octets - units of 8 bits - more precise than octets
![[Pasted image 20240913093106.png]]
- payload of frames are generally limited to 1500 octets, 46 minimum (MTU - maximum transmissible unit)
	- destination mac address - 6 octets (48 bits), shows where frame is being sent to
	- source mac address - 6 octets, shows where frame came from
	- type/length - 2 octets, what ethertype
	- payload - self explanatory
	- Frame check sequence - cyclic redundancy check to see if payload has any issues/errors
		- FCS computed at destination to see if it matches to check
- Preamble/Start Frame Delimiter - 101011 - demonstrates when the destination mac address starts - to sync up timing
- interpacket gap - pause between packets

 Interfaces are configured with a burned-in mac address - will not interrupt the host for destination mac addresses that do not match interface
	 except: broadcast and multicast - multiple devices, promisc

#### MAC Addresses (Media Access Control)

![[Pasted image 20240913093140.png]]hyphens are technically more correct, but sometimes separated by colons - canonical/non-canonical format

#### Switches

switches create a broadcast domain from collection of individual LAN segments
- basically, connects the different collision domains of lan systems
- full duplex - send and receive at the same time 
- some switches are routers - aka layer 3

#### Key Points:

- **Ethernet Diameter**: The "diameter" of an Ethernet network is the farthest distance between two devices on the network. This distance matters because it affects how long it takes for signals to travel between devices.
    
- **Maximum Network Size**: Ethernet has a limit on how big the network can be. This is measured in "bit times," which are the amount of time it takes to send a single bit of data. The maximum distance is set so that the round-trip time for a signal between the two farthest devices is 464 bit times.
    
- **Collisions and Jam Signal**: Sometimes, two devices try to send data at the same time, causing a collision. When this happens, each device sends a "jam signal" (which is 48 bits long) to make sure the other device knows a collision occurred.
    
- **Slot Time**: After accounting for the round-trip time (464 bits) and the jam signal (48 bits), we get 512 bits total. The time it takes to send 512 bits is called the "slot time," which is about 51.2 microseconds. This is an important concept in Ethernet.
    
#### Why Slot Time is Important:

- **Collision Detection**: If a device has been sending data for one slot time (512 bits), it knows there won’t be a collision anymore. This is because the slot time is long enough for any other device on the network to notice that someone else is sending data, so they'll wait their turn.
    
- **Minimum Packet Size**: Ethernet has a minimum packet size, which is 64 bytes. This is important because if a packet is too small, the sender might not realize a collision happened. So, even if you only need to send a small amount of data (like a 40-byte TCP acknowledgment), you have to pad it out to meet the minimum 64-byte size.
    

#### In Simple Terms:

Ethernet sets a time limit (slot time) that allows devices to detect when someone else is sending data. If a device has been sending data for this slot time, it knows it's safe and there won’t be a collision. If there’s a collision early on, both devices stop and try again later.

To prevent small packets from causing problems, Ethernet requires all data packets to be at least 64 bytes. If your data is smaller, it gets padded with extra bits so the system works properly.

empty forwarding tables - will do flooding = send to all other ports until data is collected, then switch to learning algorithm
- will keep track of source and interface, then when data is sent to the same MAC address it will ensure only to send through that specific interface