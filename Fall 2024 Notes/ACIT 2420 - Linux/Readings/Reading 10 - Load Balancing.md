- Load balancing is the process of distributing traffic among multiple servers to improve a service or application's performance and reliability.
- Load balancing is the practice of distributing computational workloads between two or more computers.
- On the Internet, load balancing is often employed to divide network traffic among several servers. 
	- This reduces the strain on each server and makes the servers more efficient, speeding up [performance](https://www.cloudflare.com/learning/performance/why-site-speed-matters/) and reducing [latency](https://www.cloudflare.com/learning/performance/glossary/what-is-latency/).
- Load balancing is essential for most Internet applications to function properly.

![[Pasted image 20241125204540.png]]
![[Pasted image 20241125204547.png]]
- By dividing user requests among multiple servers, user wait time is vastly cut down

### How does load balancing work?
- Load balancing is handled by a tool or application called a **load balancer.**
- A load balancer can be either hardware-based or software-based.
	- Hardware load balancers require the installation of a dedicated load balancing device
	- software-based load balancers can run on a server, on a [virtual machine](https://www.cloudflare.com/learning/cloud/what-is-a-virtual-machine/), or in the [cloud](https://www.cloudflare.com/learning/cloud/what-is-the-cloud/).
- [Content delivery networks (CDN)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) often include load balancing features.
- When a request arrives from a user, the load balancer assigns the request to a given server, and this process repeats for each request
- Load balancers determine which server should handle each request based on a number of different algorithms. 
	- These algorithms fall into two main categories: **static** and **dynamic**.

### Static Load Balancing Algorithms

#### Static
- Static load balancing algorithms distribute workloads without taking into account the current state of the system.
- A static load balancer will not be aware of which servers are performing slowly and which servers are not being used enough
- assigns workloads based on a predetermined plan. 
- Static load balancing is quick to set up, but can result in inefficiencies.

#### Dynamic
- Dynamic load balancing algorithms take the current availability, workload, and health of each server into account.
- They can shift traffic from overburdened or poorly performing servers to underutilized servers, keeping the distribution even and efficient
- dynamic load balancing is more difficult to configure.
	-  A number of different factors play into server availability: the health and overall capacity of each server, the size of the tasks being distributed, and so on.

### Where is load balancing used?
- load balancing is often used with web applications. 
- Software-based and cloud-based load balancers help distribute Internet traffic evenly between servers that host the application.
- Some cloud load balancing products can balance Internet traffic loads across servers that are spread out around the world, a process known as [global server load balancing (GSLB)](https://www.cloudflare.com/learning/cdn/glossary/global-server-load-balancing-gslb/).
- Load balancing is also commonly used within large [localized networks](https://www.cloudflare.com/learning/network-layer/what-is-a-lan/), like those within a data center or a large office complex.
- Traditionally, this has required the use of hardware appliances such as an application delivery controller (ADC) or a dedicated load balancing device. Software-based load balancers are also used for this purpose.

### Server monitoring
- Dynamic load balancers must be aware of server health: their current status, how well they are performing, etc.
- Dynamic load balancers monitor servers by performing regular server health checks.
- If a server or group of servers is performing slowly, the load balancer distributes less traffic to it. 
- If a server or group of servers fails completely, the load balancer reroutes traffic to another group of servers, a process known as **"failover."**

### What is failover?
- Failover occurs when a given server is not functioning and a load balancer distributes its normal processes to a secondary server or group of servers.
- Server failover is crucial for reliability: if there is no backup in place, a server crash could bring down a website or application. 
	- It is important that failover takes place quickly to avoid a gap in service.