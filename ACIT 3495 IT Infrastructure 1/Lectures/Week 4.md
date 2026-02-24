### docker images
- search for an image `docker search ____`
- pull selected image `docker pull ____`
- execute `docker run ____`

### docker architecture
- need `runc` for docker
- control groups - cgroups
	- can control containers that try to abuse resources
- namespaces
	- pid, net, 
- layered file system
	- container image is made up of a stack of immutable or read only layers
	- when docker makes a container from an image, it adds a writeable container layer on top of this stack 

### creating an image
- interactively building a container 
- `docker commit <container>myubuntu:v10`
- `docker image history myubuntu:v10`
#### creating from a file
![[Pasted image 20260129085812.png]]

### docker compose
- ![[Pasted image 20260129090204.png]]