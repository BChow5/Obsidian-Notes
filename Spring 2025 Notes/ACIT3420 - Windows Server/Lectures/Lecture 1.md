### server
- a computer that provides services and a client is a computer that requests services
- network made up of servers and clients is known as a client/server network 
- a server based network is the best network for sharing resources and data
	- while providing centralized network security for those resources and data
- networks using Windows servers are typically client/server networks
- server vs desktop 
	- reliability, availability, scalability, usability, manageability 
- types of servers
	- file share, web server, database server, application server, etc.  
- any computer can be a server but an enterprise will use server level hardware 

#### Server Roles
- windows servers use "roles" to define what services the server provides 
- a server can have multiple roles installed
- when you determine hardware and software needs, look at the role the computer needs to fill and load the computer will be placed under 
### Server Hardware Selection 
- when choosing server hardware, keep the following in mind:
	- performance - servers are intended to provide network services to many users
	- availability - if the server fails or becomes unavailable, the issue will affect multiple people
	- cost - how to balance available budget with the foals of performance and availability 
![[Pasted image 20250111112320.png]]

### Primary subsystems
- processor
- memory
- storage
- network
- failure in any of these subsystems can cause the entire system to fail
- they can also cause a bottleneck that can affect performance of the entire system 

### difference between full control and change
- this is about file sharing on the PC in the advanced settings 
- full control lets you take owner of the folder
- change cannot take owner of the folder 
- this is the only difference 

### Processor
- processor are typically 64 bit, which can process faster than 32 bit processor
	- 32 bit = 4 gigabytes of RAM
	- 64 bit = 1 terabyte of RAM
- a processor can only work on a single provess/thread at a time
- time slicing enables multiple threads and applications to be running at the same time 
- a multicore processor enables multiple threads/application instructions to be worked on at the same time
- CICS - Complex Insutrcion Set Computer (single instruction can execute several low level operation)
- RISC - one instruction can be executed within one clock cycle
- ARM (advanced RISC) - smaller size, reduced complexity, lower power

### Memory (ECC)
- Random Access Memory (RAM) is the computer's short term or temporary memory
- it stores instruction and data that the processor accesses directly
- if you have more RAM, you can load more instruction and data from the disks
- having sufficient RAM can be one of the main factors in your computer's overall performance
- rebooting can help fix problems because it reloads the cache
	- many problems can be fixed this way 
- ECC is server level hardware
	- ECC built onto the RAM 
	- chip amounts divisible by 3 or 5
	- ECC has at least 1 extra chip for verifying data 
- downtime is so that servers can still be rebooted to correct any issues 
![[Pasted image 20250109093422.png]]

### Storage (local/network storage (NAS,DAS,SAN,S2D))
- Need to understand Read levels
- traditionally, hard drives are half electronic/half mechanical devices that store magnetic fields on rotating platters
- solid state drives are electronic devices with no mechanical components
- personal computers typically only have local storage consisting of internal hard drives
- servers may also connect to external storage through a Network Attached Storage (NAS) or Storage Are Network (SAN)
- enterprise solutions
	- NAS, DAS, SAN
- S2D is important to know
	- software defined storage solution
	- belongs to Microsoft 

### Network Interface controller (NIC)
- a network connection enables a deceive to communicate with servers or clients
- more devices include one or more network interface cards 
- Speeds of today’s network cards are 1000 Mbit/second (1GbE) or 10000Mbit/second(10GbE), while a typical speed for servers is 10 Gbit/second or faster such as 10/25/100GbE (RDMA)
- enterprise level will use fiber channel
![[Pasted image 20250109094130.png]]

### Motherboard
- motherboard is the main printede circuit board that brings these four sub systems together
- the processory plugs in or connects to the motherboard or system board enabling communication with the rest of the system
- the motherboard provides the electrical connections enabling components of the system to communicate 
- riser card provides more expansion slots
![[Pasted image 20250109094300.png]]
![[Pasted image 20250109094423.png]]

### Power Supply and Cases
- at least two (Fault Tolerance)
- A case provides an enclosure that helps protect system components that are inside
- a case with the powre supplies and additional fans are usually designed to provide a fair amount of airflow through the system to keep the system cool
- the power supply supplies electrical power to the motherboard and components 
- hot patching is a new feature of Windows Server 2025 
![[Pasted image 20250109094640.png]]
- NIC Team
	- combining multiple ports to one
	- if one port fails, the others can still provide service
- servers don't support HDMI 
	- lots of servers don't have a monitor also 
- you can manage the servers through a Server Manager 

### Ports and connectors
- ports are plug sockets that enable to to connect peripheral devices to your computer
- types of ports/connectors
	- ethernet
	- parallel
	- PS/S
	- serial
	- NVMe
	- VGA
	- USB
	- SCSI
	- HDMI
	- Audio
	- SAS
	- 

### KVM
- a keyboard, video, mouse (KVM) switch is a hardware device that can be used to control more than one computer while using a single keyboard, monitor, and mouse
- for businesses, KVM switches provide cost effective access to multiple servers
	- home users can save space using a KVM switch to connect multiple computers to one keyboard, monitor, and mouse 
- could also use something like Zabbix to manage multiple servers 
![[Pasted image 20250109095242.png]]

### Why would a company need 2 ISPs
- for high availability 
	- in case one goes down, you have a backup 
- e.g. having Telus and Shaw
![[Pasted image 20250109095335.png]]

### BIOS/UEFI
- Basic Input/Output System (BIOS) are the instructions that control most of the computer's input/output functions 
- BIOS functions include communicating with disks, RAM, and the monitor kept in the System Read Only Memory (ROM) chips
- by having instruction (software) written on the BIOS, the system already knows how to communicate with some basic components such as a keyboard, and how to read basic disks such as IDE drives
- the BIOS also looks for additional ROM chips that may be on the motherboard or expansion cards
- flashing the BIOS is the process of updating your system ROM BIOS 

### Configure a new server 
![[Pasted image 20250111113125.png]]