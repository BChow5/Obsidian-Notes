## What is SSH
* <mark style="background: #ABF7F7A6;">Secure Shell Protocol (SSH)</mark> is a method for securely sending commands to a computer over an unsecured network 
* SSH uses cryptography to authenticate and encrypt connections
* allows for tunneling or port forwarding where data packets are able to cross networks that they wouldn't normally be able to cross
* often used for controlling servers remotely for managing infrastructure, and transferring files

```ad-summary
<mark style="background: #FFB86CA6;">Remote Encrypted Connections</mark>
* SSH sets up a connection between a user's device and a far away machine like a server

<mark style="background: #FFB86CA6;">Ability to tunnel </mark>
* Tunneling is a method for moving packets across a network using a protocol or path they wouldn't be able to use normally

```

### How does SSH work?
#### TCP/IP
* SSH runs on top of TCP/IP protocol suite
* <mark style="background: #ABF7F7A6;">TCP</mark> - Transmission Control Protocol
* <mark style="background: #ABF7F7A6;">IP</mark> - Internet Protocol 
* TCP/IP pairs these two protocols to format, route, and deliver packets 
	* IP indicates info like which IP address while TCP indicates which port the packet should go to 
* TCP is a transport layer protocol
	* involved with the transportation and delivery of data packets 
	* SSH is often used with TCP/IP

#### Public Key Cryptography
- SSH is "secure" because it incorporates encryption and authentication via Public Key Cryptography
- <mark style="background: #ABF7F7A6;">Public Key Cryptography</mark> - way to encrypt data or sign data with two different keys
	- public key available for anyone to use and private key that is secret only to the owner
	- the two corresponding keys establish the key owners identity 
- In SSH connections, both sides have public/private key pairs and each side authenticates with the keys
	- this is different from HTTPS which only verifies the identity of the web server 

```ad-example
title: Example of SSH Tunneling
For example, imagine an administrator wants to make a change on a server inside a private network they manage, and they want to do so from a remote location. However, for security reasons, that server only receives data packets from other computers within the private network. The administrator could instead connect to a second server within the network — one that is open to receiving Internet traffic — and then use SSH port forwarding to connect to the first server. From the first server's perspective, the administrator's data packets are coming from inside the private network.

```

### What is SSH used for?
* Remotely managing servers, infrastructure, and employee computers
* Securely transferring files (more secure than FTP)
* Accessing services in the cloud without exposing a local machine's port to the internet
* Connecting remotely to services in a private network
* Bypassing firewall restrictions

```ad-note
title: Note - SSH Ports
* SSH uses port 22 as a default port
* firewarlls may block access to certain ports on servers behind the firewall but they leave port 22 open
```

#### How does SSH contrast with other protocols for tunneling?
* One of the main differences between SSH and other tunneling protocols is the [OSI layer](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/) at which they operate
* [GRE](https://www.cloudflare.com/learning/network-layer/what-is-gre-tunneling/), IP-in-IP, and [IPsec](https://www.cloudflare.com/learning/network-layer/what-is-ipsec/) are all [network layer](https://www.cloudflare.com/learning/network-layer/what-is-the-network-layer/) protocols. As such, they are not aware of ports (a transport layer concept), instead operating between IP addresses.
* SSH's exact OSI layer is not strictly defined, but most sources describe it as a [layer 7](https://www.cloudflare.com/learning/ddos/what-is-layer-7/)/application layer protocol.
* <mark style="background: #ABF7F7A6;">UDP</mark> is a "best-effort" transport protocol sending packets without ensuring their delivery — which makes it faster but sometimes results in packet loss. 
* Although TCP is slower than UDP, it guarantees delivery of all packets in order, and is therefore more reliable.