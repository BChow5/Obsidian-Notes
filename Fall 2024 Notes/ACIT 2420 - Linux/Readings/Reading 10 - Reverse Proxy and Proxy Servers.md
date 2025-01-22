### What is reverse proxy?
- A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers.
- Reverse proxies are typically implemented to help increase [security](https://www.cloudflare.com/learning/security/what-is-web-application-security/), [performance](https://www.cloudflare.com/learning/performance/why-site-speed-matters/), and reliability.

### What is a proxy server?
- A forward proxy, often called a proxy, proxy server, or web proxy, is a server that sits in front of a group of client machines.
- When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.

![[Pasted image 20241125211251.png]]
- When a forward proxy is in place, A will instead send requests to B, which will then forward the request to C. C will then send a response to B, which will forward the response back to A.

#### Why use a forward proxy?
- **To avoid state or institutional browsing restrictions** - Some governments, schools, and other organizations use firewalls to give their users access to a limited version of the Internet.
	- A forward proxy can be used to get around these restrictions, as they let the user connect to the proxy rather than directly to the sites they are visiting.
- **To block access to certain content**
	- proxies can also be set up to block a group of users from accessing certain sites.
	- e.g. a school network might be configured to connect to the web through a proxy which enables content filtering rules, refusing to forward responses from Facebook and other social media sites.
- **To protect their identity online**
	- Only the IP address of the proxy server will be visible.

### How is reverse proxy different?
- A **reverse proxy** is a server that sits **in front of one or more web servers,** intercepting requests from clients.
- This is different from a **forward proxy**, where the proxy sits in **front of the clients.**
- With a reverse proxy, when clients send requests to the origin server of a website, those requests are intercepted at the [network edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/) by the reverse proxy server. 
	- The reverse proxy server will then send requests to and receive responses from the origin server.
- A simplified way to sum it up would be to say that a forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. 
- On the other hand, a reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.

![[Pasted image 20241125211759.png]]
- Typically all requests from D would go directly to F, and F would send responses directly to D. With a reverse proxy, all requests from D will go directly to E, and E will send its requests to and receive responses from F. E will then pass along the appropriate responses to D.

#### Benefits of reverse proxy
- **[Load balancing](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/)**
	- the site can be distributed among a pool of different servers, all handling requests for the same site.
	- a reverse proxy can provide a load balancing solution which will distribute the incoming traffic evenly among the different servers to prevent any single server from becoming overloaded.
- **Protection from attacks**
	- With a reverse proxy in place, a web site or service never needs to reveal the IP address of their origin server(s)
	- makes it much harder for attackers to leverage a targeted attack against them, such as a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/)
- **[Global server load balancing](https://www.cloudflare.com/learning/cdn/glossary/global-server-load-balancing-gslb/) (GSLB)**
	- a website can be distributed on several servers around the globe and the reverse proxy will send clients to the server that’s geographically closest to them.
	- This decreases the distances that requests and responses need to travel, minimizing load times.
- **Caching**
	- A reverse proxy can also [cache](https://www.cloudflare.com/learning/cdn/what-is-caching/) content, resulting in faster performance
	- The proxy server can then cache (or temporarily save) the response data.
		- Subsequent Parisian users who browse the site will then get the locally cached version from the Parisian reverse proxy server, resulting in much faster performance.
- **SSL encryption**
	- [Encrypting](https://www.cloudflare.com/learning/ssl/what-is-encryption/) and decrypting [SSL](https://www.cloudflare.com/learning/ssl/what-is-ssl/) (or [TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/)) communications for each client can be computationally expensive for an origin server.
	- A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.

### How to implement a reverse proxy?
- Some companies build their own reverse proxies, but this requires intensive software and hardware engineering resources, as well as a significant investment in physical hardware. 
- One of the easiest and most cost-effective ways to reap all the benefits of a reverse proxy is by signing up for a CDN service. 
	- For example, the [Cloudflare CDN](https://www.cloudflare.com/application-services/products/cdn/) provides all the performance and security features listed above, as well as many others.