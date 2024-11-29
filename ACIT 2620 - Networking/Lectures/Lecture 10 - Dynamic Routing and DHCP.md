### DHCP
- **Attempt to Obtain an Address from a DHCP Server (Windows):**
    - When a Windows device is configured to obtain an IP address automatically, it will broadcast a DHCP discovery message to find a DHCP server.
    - If a DHCP server is available, the device will receive an IP address and other configuration details.
- **Fallback if No DHCP Server is Found:**
    - If the device cannot locate a DHCP server after repeated attempts, it uses **APIPA** (Automatic Private IP Addressing).
    - **APIPA Range:** The device assigns itself an IP address from the special reserved range `169.254.0.1` to `169.254.255.254` with a subnet mask of `255.255.0.0`.
    - These addresses are only valid for local communication within the same network segment. They cannot be used to communicate outside the local network, such as accessing the internet.
- **Special Behavior:**
    - If the device assigns an APIPA address, it continues to periodically check for a DHCP server. If a DHCP server becomes available, it will replace the APIPA address with one assigned by the DHCP server.

#### New node first joining a network 
- when a node first joins a network
	- it sends out a broadcast message (called **discover**)
	- and it will reach the DHCP server if there is one
	- DHCP server responds with an **offer** in unicast 
		- offer contains the ip address to use and all accompanying settings
			- default gateway, DNS, etc.
	- node sends back an acceptance message (**request**)
	- sever then acknowledges it  
- Discover, offer, request, acknowledge 
	- all of this is a lease 
- when time elapses, they will need to renew their lease
	- send another request to continue using that address 

#### What goes into the offer?
- IP address and subnet mask
	- subnet
	- range of addresses
- gateway
	- routers setting 
	- location of the default gateway 
- DNS

#### DHCP 
- at least one of the servers interface needs to be part of the subnet that we're configuring 
- e.g. 10.20.30.0/24
	- then one of the interfaces of the DHCP server needs to have an address from that range 

### Dynamic routing
- routes have a cost to the destination based on how many hops
	- always choose the shortest route
- if the cost is the same, the router may remove a duplicate 
	- could also remove the one with the higher cost 
- we rely on the routing table to tell us the path to go

#### Routing Types
- default route (last resort)
	- Specifies the gateway to use when the routing table does not contain a path for the destination network.  
	- Commonly points to the next router in the path to the ISP  
	- Identified by the word “default” or subnet “0.0.0.0/0” in the destination value field
- connected
	- directly attached, local-network (to the router) addresses
	- identified in the routing table with scope link
	- these routes are automatically updated whenever the interface is reconfigured or shutdown 
- static
	- manually configured routes
	- identified in the routing table with "proto static"
- dynamic
	- automatically created and maintained by routing protocols
	- identified in the routing table by the name of the protocol that created them
- routers will make decisions only based on what destinations they know about 
- the metric assigned to each route is the cost 
- local network if there is no next hop since that means the router is directly connected 

### Routing Protocols

![[Pasted image 20241115154916.png]]
- diagram to show that internet is under control of different ISP 
- working between 2 different ISP require a different routing protocol 
- **distance vector protocol**
	- immediate neighbors exchanging what they know and adding that information to their routing tables 

#### Routing protocols: internet structure

#### Routing protocols: learning routes