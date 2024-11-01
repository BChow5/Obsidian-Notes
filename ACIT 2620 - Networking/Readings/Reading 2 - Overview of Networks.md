- Local Area Networks, or <mark style="background: #ABF7F7A6;">LANs</mark>, are the “physical” networks that provide the connection between machines within, say, a home, school or corporation.
- <mark style="background: #ABF7F7A6;">IP</mark> or Internet Protocol is the layer that provides an abstraction for connecting multiple LANs into the Internet.
- TCP deals with the transport and connections and actually sending user data

#### Layers
-  LAN, IP, and TCP are often called **layers**
	- they constitute the Link Layer, Internetwork Layer, and Transport Layer 
-  <mark style="background: #ABF7F7A6;">Four Layer Model</mark> for networks
	- Link Layer
	- Internetwork Layer
	- Transport Layer
	- Application Layer 
- layer in this context is a programming interface or library
	- layers communicate directly only with the two layers immediately above and below
- An application hands off a chunk of data to the TCP library, which in turn makes calls to the IP library, which in turn calls the LAN layer for actual delivery. 
	- An application does _not_ interact directly with the IP and LAN layers at all.

##### LAN Layer
- The LAN layer is in charge of actual delivery of packets, using LAN-layer-supplied addresses.
- often conceptually subdivided into the **physical layer**
	- e.g. dealing with the analog, electrical, optical, etc.
- logical layer that describes all the digital operations on packets
- The physical layer is generally of direct concern only to those designing LAN hardware
- the kernel software interface to the LAN corresponds to the logical LAN layer.\
- ![[Pasted image 20240909231838.png]]
- This LAN physical/logical division gives us the Internet <mark style="background: #ABF7F7A6;">Five-Layer Model.</mark>

### Data Rate, Throughput and Bandwidth
-  Any network connection has a data rate
- <mark style="background: #ABF7F7A6;">Data Rate</mark>: the rate at which bits are transmitted.
	- data rate can vary with time on wifi
- <mark style="background: #ABF7F7A6;">Throughput</mark> - the overall effective transmission rate
	- takes into account think like transmission overhead, protocol inefficiencies 
- <mark style="background: #ABF7F7A6;">Bandwidth</mark> - could refer to either data rate or throughput but it is mostly used as a synonym for Data Rate 
- In discussions about TCP, the term <mark style="background: #ABF7F7A6;">Goodput</mark> is sometimes used to refer to what might also be called “application-layer throughput”
	- the amount of usable data delivered to the receiving application.
	- retransmitted data is counted only once when calculating goodput but might be counted twice under some interpretations of “throughput”.
- Data rates are generally measured in kilobits per second (kbps) or megabits per second (Mbps)

### Packets
- Packets are modest-sized sequences of bytes, transmitted as a unit through some shared set of links
- packets need to be prefixed with a **header** containing delivery information
- in datagram forwarding, the header contains a <mark style="background: #ABF7F7A6;">destination address</mark>
- headers in networks using <mark style="background: #ABF7F7A6;">virtual circuit forwarding</mark> instead contain an identifier for the connection
- almost all networking is packet based

![[Pasted image 20240910194329.png]]
- at the LAN layer, packets can be viewed as the imposition of a buffer and addressing) structure on top of low level serial lines 
- additional layers then impose additional structure
- packets are often referred to as <mark style="background: #ABF7F7A6;">frames</mark> at the LAN layer and <mark style="background: #ABF7F7A6;">segments</mark> at the transport layer 
	- LAN layer = frames
	- Transport Layer = segments 

##### Max packet size 
-  ethernet allows a max of 1500 bytes of data
- TCP/IP packets originally often held only 512 byes of data
- Token Ring packets could contain up to 4 kB of data
- ATM (Asynchronous Transfer Mode) protocol uses 48 bytes of data per packet
- potential issue from forwarding large packets through a small-packet LAN 

##### Headers
- generally each layer adds its own header 
	- ethernet headers are typically 14 bytes
	- IP headers 20 bytes
	- TCP headers 20 bytes
- In datagram-forwarding networks the appropriate header will contain the address of the destination and perhaps other delivery information.
- **routers** or **switches** will then try to ensure that the packet is delivered to the requested destination.

### Datagram Forwarding
- In the datagram-forwarding model of packet delivery, packet headers contain a destination address
	- it's up to the intervening switches or routers to look at this address and get the packet to the correct destination 

#### forwarding tables 
- each switch is provided a <mark style="background: #ABF7F7A6;">Forwarding Table</mark> of **destination** and **next hop** pairs 

##### Next hop
- when a packet arrives, the switch looks up the destination address in its Forwarding Table and finds the **next hop** information 
	- <mark style="background: #ABF7F7A6;">next hop</mark> is the immediate neighbor address to which the packet should be forwarded in order to bring it closer to the final destination 
- The next hop value in a forwarding table is a single entry
- each switch is responsible for only one step in the packet’s path

##### Destination
-  destination entries in forwarding tables do not have to correspond exactly with the packet destination addresses 
- for IP routing, the table destination entries will correspond to prefixes of IP addresses 
	- saves a lot of space
- fundamental requirement that the switch can perform a lookup operation
	- using its forwarding table and the destination address in the arriving packet to determine the next hop

#### Datagram Forwarding
- A central feature of datagram forwarding is that each packet is forwarded “in isolation”
	- the switches involved do not have any awareness of any higher-layer logical connections established between endpoints
	- called <mark style="background: #ABF7F7A6;">stateless forwarding</mark>
-  alternative to datagram forwarding is <mark style="background: #ABF7F7A6;">virtual circuits</mark>
- in virtual circuits, each router maintains state about each connection passing through it
	- different connections can be routed differently 
- if packet forwarding depends on per connection information (e.g. both TCP port numbers) then it is not datagram forwarding
- In theory, IP routing can be done based on the destination address and some **quality-of-service** information, allowing, for example, different routing to the same destination for high-bandwidth bulk traffic and for low-latency real-time traffic.
	-  In practice, most Internet Service Providers (ISPs) ignore user-provided quality-of-service information in the IP header, except by prearranged agreement, and route only based on the destination.

#### Switches 
- switching devices acting at the LAN layer and forwarding packets based on the LAN address are called <mark style="background: #ABF7F7A6;">switches</mark>
- devices acting at the IP layer and forwarding on the IP address are called <mark style="background: #ABF7F7A6;">routers</mark>

#### Default Entry
- In IP routers within end-user sites it is common for a forwarding table to include a catchall **default** entry
	- any IP address matching the default entry it routed out to the internet 
- Default entries make sense only when we can tell by looking at an address that it does not represent a nearby node
- common in IP networks because an IP address encodes the destination network, and routers generally know all the local networks
- rare in Ethernets, because there is generally no correlation between Ethernet addresses and locality

### Topology
- In the network diagrammed in the previous section, there are no loops; graph theorists might describe this by saying the network graph is **acyclic**, or is a **tree**.
	-  In a loop-free network there is a unique path between any pair of nodes. The forwarding-table algorithm has only to make sure that every destination appears in the forwarding tables
	- the issue of choosing between alternative paths does not arise.
- However, if there are no loops then there is no **redundancy**
	- any broken link will result in partitioning the network into two pieces that cannot communicate.
	- once we start including redundancy, we have to make decisions among the multiple paths to a destination
- ![[Pasted image 20240910213448.png]]
	- Some sort of protocol must exist to provide a mechanism by which S1 can make the choice

### Traffic Engineering
- We will use the term <mark style="background: #ABF7F7A6;">traffic engineering</mark> to refer to any intentional selection of one route over another, or any elevation of the priority of one class of traffic
- The route selection can either be directly intentional, through configuration, or can be implicit in the selection or tuning of algorithms that then make these routes
- With pure datagram forwarding, used at either the LAN or the IP layer, the path taken by a packet is determined solely by its destination
	- traffic engineering is limited to the choices made between alternative paths.
- We have already, however, suggested that datagram forwarding can be extended to take quality-of-service information into account
	- this may be used to have voice traffic – with its relatively low bandwidth but intolerance for delay – take an entirely different path than bulk file transfers.

### Routing Loops
- A potential drawback to datagram forwarding is the possibility of a
	-  <mark style="background: #ABF7F7A6;">routing loop</mark>: a set of entries in the forwarding tables that cause some packets to circulate endlessly
- Routing loops typically arise because the creation of the forwarding tables is often “distributed”, and there is no global authority to detect inconsistencies.
	- Even when there is such an authority, temporary routing loops can be created due to notification delays.
- Routing loops can also occur in networks where the underlying link topology is loop-free