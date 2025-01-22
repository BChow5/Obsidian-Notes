### Why IPv6?
- we switch because running out of IPv4 addresses 
- The ability to scale networks for future demands requires  
- simplifies the process 
- a large supply of IP addresses and improved mobility.  
	- IPv6 combines expanded addressing with a more efficient header.  
	- IPv6 satisfies the complex requirements of hierarchical addressing.

### IPv4 Address Depletion 
- Though NAT, VLSM and CIDR were developed as workarounds and  have helped to extend the life of IPv4, the address space is nearing exhaustion

### What happened to IPv5?
- the **Internet Stream Protocol (ST)** was developed to experiment with voice, video, and distributed simulation
- newer ST2 packets used IPv5 in the header
- abandoned due to limited scope 
- although not officially known as IPv5, ST2 is considered to be the closest thing
- the next internet protocol became IPv6

### Features of IPv6
- Larger address space
- Elimination of public-to-private NAT
- Elimination of broadcast addresses
	- relies on multicasting instead
- Simplified header for improved router efficiency
- Support for mobility and security
- Many devices and applications already support IPv6
- Prefix renumbering simplified
- Multiple addresses per interface
- Address autoconfiguration
- No requirement for DHCP
- Link-local and globally routable addresses
- Multiple-level hierarchy by design
	- More efficient route aggregation
- Transition mechanisms from IPV4 to IPV6

### IPv6 Addressing Overview
- IPv6 increases the number of address bits by a factor of  4, from 32 to 128, providing a very large number of  addressable nodes
- ![[Pasted image 20241108134949.png]]

### IPv6 Address Specifics
- The 128-bit IPv6 address is written using 32 hexadecimal numbers.  
- The format is x:x:x:x:x:x:x:x, where x is a 16-bit hexadecimal field, therefore each x represents four hexadecimal digits. 
	- there is 8 groups of for hexadecimal digits 
- Example address:  
	- 2035:0001:2BC5:0000 : 0000:087C:0000:000A

#### Abbreviating IPv6 Addresses
- Leading 0s within each set of four hexadecimal digits can be omitted.  
	- 09C0 = 9C0  
	- 0000 = 0  
	- DO NOT DROP trailing zeros 
- A pair of colons (“::”) can be used, once within an address, to represent any number (“a bunch”) of successive zeros.

![[Pasted image 20241108141319.png]]![[Pasted image 20241108141335.png]]

### IPv6 Address Components
- consists of 2 parts
	- first 64 bits - a subnet/network previx
	- last 64 bits - an interface ID
![[Pasted image 20241108141509.png]]

### IPv6 Address Types
- unicast
- multicast
- anycast
![[Pasted image 20241108141546.png]]
- **Multicast** sends data to all members of a group.
- **Anycast** directs data to the nearest device within a group of devices, each having the same IP.

### IPv6 Unicast Address Scopes
- Address types have well-defined destination scopes:  
	- Link-local address  
		- used for communication within a single network segment or link (like a local subnet)
		- mainly for local communication
		- Local segment communication
	- Unique-local addresses (replaced Site-local address)  
		- intended for internal network communications, similar to private IPv4 addresses (like 192.168.x.x). They are not routable on the global internet, but they are routable within an organization.
		- Within an organization or site (corporate intranet)
	- Global unicast address  
		- globally unique and routable across the IPv6 internet. They serve as the equivalent of public IPv4 addresses and can be assigned to devices that need internet access.
* Determined by the leading digits of the subnet prefix  
* Link-local addresses—only on single link, not routed  
	* **FE80::/10 prefix**  
* Unique-local addresses—routed within private network (aka private IPv4 addresses)  
	* **FC00::/7 (still reserved) and FD00 prefix**  
* Global unicast addresses—globally routable  
	* **2001 prefix most common**
![[Pasted image 20241108141839.png]]

### IPv6 Addressing Space Overview
- Addresses that start with FF are multicast
- FC are unique local address 

| 16 Bit Prefix Hex Value      | use                                      |
| ---------------------------- | ---------------------------------------- |
| 0000 to 00FF                 | Unspecified,  Loopback, IPv4-compatible  |
| 0100 to 01FF                 | Unassigned (0.38 % of IPv6 space)        |
| 0200 to 03FF                 | NSAP (Network Service Access Provider)   |
| 0400 to 1FFF                 | Unassigned (~11% of IPv6 space)          |
| 2000 to 3FFF                 | Aggregatable global unicast  <br>(12.5%) |
| 4000 to FE7F (Huge)          | Unassigned (~75% of IPv6 space)          |
| FE80 to FEBF                 | **Link-local**                           |
| FC00 to FCFF<br>FD00 to FDFF | **Unique-local**                         |
| FF00 to FFFF                 | **Multicast**                            |
#### Special IPv6 Addresses

| IPv6 Address        | Description                                                                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| : :/0               | - All networks and used when specifying a default static  <br>route.  <br>- It is equivalent to the IPv4 quad-zero (0.0.0.0)                                     |
| : :/128             | - Unspecified address and is initially assigned to a host  <br>when it first resolves its local link address<br>- different from unknown. this is just temporary |
| : :1/128            | - Loopback address of local host  <br>- Equivalent to 127.0.0.1 in IPv4                                                                                          |
| FE80: :/10          | - Link-local unicast address  <br>- Similar to the Windows autoconfiguration IP address of  <br>169.254.x.x                                                      |
| FF00: :/8           | Multicast addresses                                                                                                                                              |
| All other addresses | Global unicast address                                                                                                                                           |
### IPv6 Unicast Addresses
![[Pasted image 20241108145806.png]]

![[Pasted image 20241108150158.png]]

### IPv4 Header vs IPv6 Header
![[Pasted image 20241108151146.png]]

#### IPv6 Header Format

| Field Name              | Size (bits) | Description                                                                                                                                                         |
| ----------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Version**             | 4           | Indicates the version of the IP protocol. For IPv6, this is always `6`.                                                                                             |
| **Traffic Class**       | 8           | Used for QoS (Quality of Service) and differentiating traffic flows (similar to the "Type of Service" field in IPv4).                                               |
| **Flow Label**          | 20          | A label used to identify packets belonging to the same flow. It can be used to optimize routing decisions or for QoS purposes.                                      |
| **Payload Length**      | 16          | The length (in bytes) of the data portion of the packet (i.e., the payload). This excludes the header length.                                                       |
| **Next Header**         | 8           | Identifies the type of the next header, which could be an upper-layer protocol (e.g., TCP, UDP, ICMPv6, etc.).                                                      |
| **Hop Limit**           | 8           | Similar to the "TTL" (Time-to-Live) field in IPv4, this field is decremented by each router the packet passes through. If it reaches zero, the packet is discarded. |
| **Source Address**      | 128         | The IPv6 address of the sending device.                                                                                                                             |
| **Destination Address** | 128         | The IPv6 address of the receiving device.                                                                                                                           |

### Explanation of Each Field

1. **Version (4 bits)**  
    Always set to `6` for IPv6, this field indicates the version of the IP protocol. In IPv4, it was `4`.
    
2. **Traffic Class (8 bits)**  
    This field allows for differentiated service and quality of service (QoS) management. It's similar to the Type of Service (ToS) field in IPv4 but with more modern QoS features.
    
3. **Flow Label (20 bits)**  
    This field is used to label packets that belong to the same flow, allowing routers to handle packets with specific needs (like QoS or load balancing) more efficiently. The flow label is typically set by the source device and may not always be used by every network device.
    
4. **Payload Length (16 bits)**  
    Specifies the length of the data section (the payload) of the packet. Since the header is fixed at 40 bytes, the payload length excludes this and only covers the data that follows the IPv6 header.
    
5. **Next Header (8 bits)**  
    This field indicates the type of the next header after the IPv6 header. This could be a protocol like TCP, UDP, or ICMPv6, or it could indicate an extension header if the packet uses any optional headers for additional functionality.
    
6. **Hop Limit (8 bits)**  
    The Hop Limit field is a countdown mechanism to avoid packets from circulating indefinitely in case of a routing loop. Each router that handles the packet decrements this value, and if it reaches zero, the packet is discarded.
    
7. **Source Address (128 bits)**  
    This field holds the IPv6 address of the sender. It’s a 128-bit address and can be used to trace the origin of the packet.
    
8. **Destination Address (128 bits)**  
    The destination address is the recipient's 128-bit IPv6 address. This is the address where the packet is intended to be delivered.

### Extension Header Order chain
![[Pasted image 20241108151624.png]]

### IPv6 Multicast Addresses
- Multicasting is at the core of many IPv6 functions and is a replacement for the broadcast address.  
- They are defined by the prefix FF00::/8

![[Pasted image 20241108151810.png]]

-   The second octet of the address contains the prefix and transient (lifetime) flags, and the scope of the multicast address.

![[Pasted image 20241108152115.png]]
- first 4 bits is for flags
	- tells if address is permanent or temporary 
	- first 2 bits is always 0
	- 3rd (P) bit tells you if it is based on a network prefix or not
	- last bit (T) =0 if permanent, 1 if temporary 
- FF0 means permanent 
- FF3 means temporary
- multicast addresses FF00: : to FF0F : : are permanent and reserved
- the length of the prefix gets incorporated into the address 

![[Pasted image 20241108152628.png]]

### Reserved IPv6 Multicast Addresses
![[Pasted image 20241108154426.png]]

- know when a multicast is permanent or temp just looking at first 4 digits
- know how to do the EUI-64
- recognize whether an address is link local or unique local or multicast 
- IPv6 header fields
- 3 types of addresses and 3 scopes of unicast addresses
- should be able to simplify or expand an address 