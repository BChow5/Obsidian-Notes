### OSI Model Recap
- (7) **Application:** domain specific messages 
	- protocol e.g. - DHCP, DNS, HTTP/HTTPS, FTP
- (6) **Presentation:** data formatting: encryption, compression, character encoding 
- (5) **Session:** organizing of messages into "visits"
- (4) **Transport:** Multiplexing, Segmentation, Re-assembly 
- (3) **Network:** Routing datagrams between networks, global addressing 
- (2) **Data-Link:** communication on shared medium 
- (1) **Physical:** signals across medium (bits


### Protocols and Layers

| **Layer**   | **Protocols**        |
| ----------- | -------------------- |
| Application | DHCP, HTTP, DNS, FTP |
| Transport   | TCP                  |
| Network     | IP, ICMP, ARP        |
| Data-Link   | Ethernet, LAN, ARP   |
| Physical    | ARP                  |

### Broadcast Domain vs Collision Domain 
- <mark style="background: #ABF7F7A6;">Broadcast Domain</mark> is a group of nodes sharing the same transmission medium
	- e.g. LAN or simply network
- when these nodes are not connected through a switch (or bridge), the network constitutes a single collision domain
- <mark style="background: #ABF7F7A6;">Collision Domain</mark> refers to a segment of a network where data packets can "collide" with one another when being sent over a shared communication medium. 
	- Collisions occur when two devices on the same network segment send data simultaneously, causing the data to become garbled and requiring retransmission. 

### Network Devices
- Network Interface Controller/Card
	- MAC address is assigned to this card 
- Hub
	- doesn't know MAC addresses
	- will just repeat signals to every other port 
- Bridge
	- connects different networks together
	- forwards traffic based on MAC addresses 
- Switch
	- forwards frames between devices using MAC addresses 
- Router
	- understands IP
	- directs data packets based on IP addresses 
- NAT Router
	- translates one IP address to another
	- usually used for private IP to public 
	- replaces the source address (private) to a public one so it can set packets to the internet 
	- **port forwarding** - used to direct incoming network traffic from the internet (or another external network) to a specific device or service within a local network (LAN). It allows external devices to communicate with services on a private network behind a router or firewall.
	- deals with the port numbers 
- Firewall
- Wireless Access Point (WAP)

### Layers and Devices
| **Layer**   | **Devices**                                       |
| ----------- | ------------------------------------------------- |
| Application |                                                   |
| Transport   | process (Software) to process. needs port numbers |
| Network     | network to network. needs IP                      |
| Data-Link   | node to node. needs MAC                           |
| Physical    | 010101                                            
- browser is a application but the tabs are a process

| **Layer**   | **Devices**                            |
| ----------- | -------------------------------------- |
| Application |                                        |
| Transport   | NAT                                    |
| Network     | router, NAT                            |
| Data-Link   | Network Interface Card, switch, bridge |
| Physical    | Network Interface Card, Hub            |
- **Hub**:
    - **Layer 1 (Physical Layer)**: A hub is a basic device that operates purely at the physical layer, repeating incoming signals to all ports. It doesn't inspect or manage data beyond basic signal transmission.
- **Bridge**:
    - **Layer 2 (Data Link Layer)**: A bridge operates at the data link layer and is used to divide a network into segments, forwarding traffic between segments based on MAC addresses.
- **Switch**:
    - **Layer 2 (Data Link Layer)** (most common): A switch operates primarily at the data link layer, using MAC addresses to forward frames between devices within the same network.
    - Some advanced switches can also operate at **Layer 3 (Network Layer)**, handling IP routing as well, known as **Layer 3 switches**.
- **Router**:
    - **Layer 3 (Network Layer)**: A router directs data packets based on IP addresses. It determines the best path for data to travel across different networks.
- **NAT Router (Network Address Translation Router)**:
    - **Layer 3 (Network Layer)**: NAT Routers operate at the network layer, translating private IP addresses to public ones and vice versa, allowing multiple devices on a private network to access the internet using a single public IP address.
- **Firewall**:
    - **Layer 3 (Network Layer)** and **Layer 4 (Transport Layer)**: Firewalls inspect traffic based on IP addresses (Layer 3) and ports (Layer 4), filtering packets and controlling traffic flow based on security rules. Some advanced firewalls (Next-Generation Firewalls) may inspect data up to **Layer 7 (Application Layer)**.
- **Wireless Access Point (WAP)**:
    - **Layer 2 (Data Link Layer)**: A WAP operates primarily at the data link layer, facilitating wireless communication between devices and the wired network by managing MAC addresses over a wireless medium (Wi-Fi).
### Network Topologies
- Bus
	- In a bus topology, all devices (nodes) are connected to a single central cable or backbone. Data is transmitted in one direction, and each device listens for data sent down the line, but only the intended recipient accepts and processes it.
- Ring
	- In a ring topology, each device is connected to two other devices, forming a circular path for data to travel. Data travels in one direction (or both directions in a dual-ring topology) until it reaches its destination.
- Star
	- In a star topology, all devices are connected to a central device, such as a hub or switch. The central device manages communication between nodes.
- Mesh
	- In a mesh topology, each device is connected to every other device on the network. There are two types of mesh:
		- **Full Mesh**: Every device is connected to every other device.
		- **Partial Mesh**: Some devices are connected to others, but not all.
- Hybrid 
	- A hybrid topology combines two or more different types of topologies. For example, a network might use a star topology in one area and a mesh topology in another, combining their respective advantages.