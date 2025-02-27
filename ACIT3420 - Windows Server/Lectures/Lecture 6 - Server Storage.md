### Configuring disks and drive types
- a physical hard drive can be designated as a basic disk or dynamic disk
	- **basic disk**
		- default storage for Windows OS
		- contain obly simple volumes 
	- **dynamic disk**
		- can be modified without restarting the windows system
		- can contain simple, spanned, striped, and mirrored volumes
- a **partition** is a defined storage space on a hard disk
	- to be usable, a hard disk needs at least one partition
- a **volume** is a partition that has been formatted into a file system 
	- disk volume requirements include a system volume and a boot volume 
- configure dynamic disk volume as:
	- simple
		- uses free space available on a single disk
	- spanned
		- extend a simple volume across multiple disks up to a max of 32
	- mirrored
		- duplicate data from one disk to a second for redundancy and fault tolerance
	- striped
		- stores data across two or more physical disks
		- written evenly to each of the physical disks 

### Selecting a partition table format
- **Master Boot Record (MBR)**
	- standard partition table
	- supports a max of 4 primary partitions per drive
		- or up to 3 primary partitions with one extended partition 
	- can partition a disk up to 2 TB
	- MBR is a single point of failure
- **GPT**
	- GPT is the successor for the MBR
	- supports a max of 128 partitions per drive
	- can partition disks up to 18 exabytes
	- use MBR for disks smaller than 2TB
	- use GPT for disks larger than 2 TB 

### File systems
- FAT, NFTS, ReFS
- **FAT**
	- basic file system
	- FAT32 to enable larger disks
	- exFAT for flash drives
	- partition size limitations 
- **NTFS**
	- metadata
	- auditing and journaling
	- security (ACLs and scryption)
- **ReFS**
	- backward compatibility support for NTFS
	- enhanced data verification and error correction
	- support for large files, directories, and volumes 
- advantages of ReFS
	- metadata integrity with checksums
	- integrity streams with user data integrity 
	- allocation of write transactional model
	- storage pooling and virtualization
	- data striping for performance and redundancy
	- disk scrubbing for protection against latent disk errors 
	- resiliency to corruptions with recovery 
	- shared storage pools across machines
	- 

### RAID
- RAID 0
- RAID 1
- RAID 6 
	- dual parity 
	- need at least 7 disks
- mirror 
	- a copy of the disk
	- need at least 5 disks 

#### How to configure RAID 
- need to buy RAID card 
	- need to specify the size 

#### Storage Pool
- just bunch of disks
- use the same size of disks 

### Selecting a file system
- FAT 
	- basic file sytem
	- parition size limitations
	- FAT32 to enable larger disks
	- exFAT developed for flash drives
- NTFS
	- metadata
	- auditing and journaling
	- compression and disk quota 
		- A **disk quota** is a **limit** set on the amount of disk space a user or group can use on a storage system
		- should do it by group 
	- 
	- security (ACLs and encryption)
- ReFS
	- backward compatibility support for NTFS
	- enhanced data verification and error correction
	- support for larger files, directories, and volumes 
	- good for hyper V

#### Benefits of ReFS
![[Pasted image 20250213091453.png]]

### Sector
- know what a sector is 
- a sector is 512 bytes
- difference between sector and cluster 
	- cluster is the smallest unit of the file 
- partitions smallest unit is a sector 
	- a cluster will have a bunch of sector 

### Selecting disk types
- EIDE
- SATA
- SCSI
- SAS
	- normally used for servers 
- SSD 
![[Pasted image 20250217155014.png]]

### What is RAID
- RAID
	- combines multiple disks into a single logical unit to provide fault tolerance and performance
		- depends on which RAID
	- provides fault tolerance by using
		- disk mirroring
		- parity information
	- can provide performance benefits by spreading disk I/O across multiple disks
	- should not replace server backups 
- should use the same connectors so the speeds are the same 
	- some disks can be faster or slower 
- same disk, disk type, size, and brand
	- but different batch 

#### RAID 0
- better performance but no fault tolerance 
- striped with no mirror or parity
![[Pasted image 20250213092101.png]]

#### RAID 1
- no performance increase but best fault tolerance
- mirrored drives
- ![[Pasted image 20250213092224.png]]

#### RAID 5
- striped parity 
- can improve read performance but not write performance 
- does not improve write because it needs to calculate parity when writing
![[Pasted image 20250213092234.png]]

#### RAID 6
- 2 copies of parity 
![[Pasted image 20250213092448.png]]

#### RAID 1 + 0
- each pair of disks is mirrored and then the mirrored disks are striped
- can improve fault tolerance and read and write performance
- costly 
![[Pasted image 20250213092429.png]]

### Overview of enterprise storage solutions
- when planning storage for Windows Server 2019/2022, you need to determine how many servers will access the disks and multiple options exist
	- **Direct Attached Storage (DAS)**
		- cheaper 
	- **Network Attached Storage (NAS)**
		- cheaper for sharing file
	- **Storage Area Network (SAN)**
		- most expensive 
		- uses fiber channels
- understand **file level** and **block level**
	- #finalQ
	- **file level storage**
		- stores data in a hierarchical file and folder structure
		- files are managed by a file system
	- **block level storage**
		- stores data in fixed sized boxes without a predefined file structure
			- usually used for databases and high performance applications
			- each storage block has a unique identifier 
- these options differ in type as well as access technologies and usage scenarios
- the right choice is critical to the overall success of the storage solution 

#### Direct Attached Storage (DAS)
- technology that is available in nearly every server
- can include
	- physical disk located inside the server
	- connection to an external array
	- connections to USB disks or alternative connectors
- advantages 
	- easy to configure
	- inexpensive 
- disadvantages
	- isolated because the disks are attached to a single server
	- less flexibility
	- scalability limits 
![[Pasted image 20250217160550.png]]

#### Networked Attached Storage (NAS)
- file level data storage that is connected to the server over a computer network to provide shared drives or folders
	- usually using Server Message Block (SMB) or Network File system (NFS)
- advantages
	- relatively inexpensive centralized storage
	- easy to configure
- disadvantages
	- slower access time
	- not appropriate in many enterprise scenarios 
![[Pasted image 20250218213024.png]]

#### Storage Area Network (SAN)
- enterprise centralized storage solution
- high availability and max flexibility
- allows systems to attach to the storage in the SAN and presents the drives to the server as if they were locally attached
- block level access to centralized storage 
- advantages
	- storage can be accessed by multiple server simultaneously with robust and fast processing
	- easily expandable
	- centralized storage with high redundancy
- disadvantage
	- most expensive
	- requires specialized admin skills 
![[Pasted image 20250218213048.png]]

#### Overview comparing storage solutions
- DAS
	- least complex
	- low set up costs
- NAS
	- best solution in specific situations
	- often used to compliment DAS and SAN solutions
- SAN
	- highest level of performance
	- provides most features

### FC Components
- components
	- SAN
	- physical or virtual system containing an HBA card (FC or iSCSI)
	- fiber channel switch
- fiber channel over ethernet (FCoE)
	- provides benefits of fiber channel in more cost effective manner
	- easier to manage in single network topology
	- no specialized training needed

### Comparing storage technology
- you can arrange data on a disk in two ways: block level or files 
- **block level storage**
	- high performance
	- often associated with SANs
	- Presents Logical Unit Numbers (LUNs) to the server
	- not the most cost effective
- file storage level
	- delivered via NAS
	- uses common file level protocols
	- uses block level storage in backend

### iSCI SAN
- iSCI is a protocol that supports access to SCSI based storage over standard TCP/IP network (local network but it runs over internet protocol)
	- typically relies on standard Ethernet network architecture
	- could use specialized hardware such as HBA or network switches
	- uses TCP/IP and TCP port 3260 to communicate
- components
	- IP network
	- iSCI targets: servers that run on the storage device 
	- iSCI initiatiors: software or host adapter that provides access to targets 
	- IQN: globally unique identifier used to address initiators and targets on an iSCI network

#### iSCI target server
- available as role in Windows server 2019/2022
- provides different types of functionality
	- network or diskless boot
	- server application storage
	- non-microsoft platforms
	- lab environments
- features
	- authentication
	- query initiator computer for ID
	- virtual hard disks
	- scalability and manageability 

#### iSCI initiator
- runs as an operating system service
- installed by default in windows server 2008 and later
- only requires the service to be started before configuring the initiator to connect to iSCI target 

### Configuring storage pools and storage spaces
- storage spaces (virtual disks) is a storage virtualization feature built into Windows server 2019/2022 and Windows 10
- the storge spaces feature consists of two components:
	- storage pools are a collection of any type and size of physical disks (JBOD) aggregated **into a single logical disk, allowing you to manage the multiple physical disks as a single disk** 
	- these **storage pools can then be segmented into multiple storage spaces**, which are then formatted with a file system and can be used just like any other regular disk on your computer 
- new disks (internal/external) can be added to the storage pool as space requirements can be added to the storage pool as space requirements increase over time 
![[Pasted image 20250223114258.png]]

### Creating Storage Pools
- one physical disk is required to create a storage pool
- at least 2 physical disks are required to create a resiliant, mirrored virtual disk
- at least 3 physical disks are required to create a virtual disk with resiliency through parity
- at least 5 physical disks are required for three way mirroring
- at least 7 physical disks are required for dual parity
- disk must be blank and unformatted
	- no volumes can exist on the disks
- disks can be attached using a variety of bus interfaces including SAS, SATA, SCSI, NVMe, and USB 

### Creating and configuring storage spaces
- when creating storage spaces, there are four resiliency types to select from (storage layout)
	- only 3 of them provide real fault tolerance
	- **simple (no resiliency):** write one copy of your data but doesn't protect against drive failures
		- requires at **least 1 drive (RAID 0)**
	- **two way mirror:** write to copies of your data to protect against a single drive failure
		- requires **at least 2 drives (RAID 1)**
	- **three way mirror:** writes three copies of your data to protect against two simultaneous drive failures
		- requires **at least 5 drives**
	- **parity:** writes data with parity information to protect against single drive failures
		- requires **at least 3 drives (RAID 5)**

### Considerations for implementing iSCI
- several factors exist which should be examined prior to the implementation of iSCI
	- network speed and performance 
	- high availability
	- security
	- infrastructure staff
	- application teams
	- vendor information
	- 

