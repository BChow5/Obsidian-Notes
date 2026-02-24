### What is HTTP?
- protocol for transfer of various data formats between server and client
	- plain text
	- hypertext
	- images
	- video
	- sound
- meta information also transferred 

#### HTTP Version History
![[Pasted image 20250121084119.png]]

### HTTP
- stateless protocol
	- each request executed independently and in isolation
	- "sessions" and tracking has to be implemented in the payload
	- unrelated to TCP being stateful
- OSI layer 7 - application layer
- default port 80 (HTTPS on Port 443)
- HTTP/1.1 and HTTP/2 use TCP/IP

#### HTTP/1.1.
- HTTP/0.9 and 1.0 were early versions of HTTP
- HTTP/1.1 saw some significant improvements
	- host header field for virtual servers
	- persistent connections
- until HTTP/1.0 connections will be closed every time a request is made
- HTTP/1.1 allows TCP connection to stay open and to be reused
	- np additional TCP handshake for subsequent HTTP requests
	- HTTP still "stateless"
	- connection: keep-alive
	- connection: close
	- pipelining is used to request multiple resources at once 
![[Pasted image 20250121085346.png]]

#### HTTP/2
- around 2010 google was working on improvements to HTTP called SPDY
	- goals were reducing load latency and increasing default security
- SPDY developed into the HTTP/2 standard
- standardized in 2015
- features
	- binary framing to split request and response headers into multiple packets
	- multiplexing
		- receive a packet of data and replicate it to create identical data and then send then separately 
	- pushing unrequested resources (pre-load initiated by server)
		- e.g. sending the CSS automatically 
	- compression of headers and payload
	- mandatory encryption (not in protocol, but required by browsers)

#### HTTP/2 multiplexing
- multiple resource requests within a single TCP connection
- can be concurrent and parallel
- unlike pipelining in HTTP/1.1, which is sequential and suffers from a head of line blocking problem
- in 1.1, it would need to keep open 3 connections and eat up bandwidth 
![[Pasted image 20250121090737.png]]

#### HTTP/3
- HTTP/2 still suffers from a potential head of line blocking problem at the TCP level
	- block at the receiving end
- HTTP/3 uses QUIC (Quick UDP Internet Connections), a transport layer protocol based on UDP
- still focused on multiplexing, but packet loss does not affect the other resources
- implementation is ongoing
	- most web browsers and servers support it
- UDP port is 443

### HTTPS old vs new
![[Pasted image 20250121091148.png]]
- UDP is faster because it doesn't do the 3 way handshake to ensure the data is received properly
- QUIC will multiplex (send several identical copies) and then send to the client 
	- so if some get dropped, it doesn't matter 
	- if multiple make it, the client will know some are identical and only use 1 of them 

### Uniform Resources
- URL
	- Uniform Resource Locator
	- refers to an existing protocol
		- http, ftp, mailto
	- points to a document on a specific server
- URN
	- Uniform Resource Name
	- globally unique, persistent identifier
		- independent of location
			- doesn't matter for the server location
- URI
	- Uniform Resource Identifier 
	- collection of URL's and URN's
	- scheme 
		- the protocol you are using
	- host
		- host name or ip number
	- port
		- TCP port number that protocol server is using
	- path
		- path and filename reference of object on server
	- parameters
		- any specific parameters that objects needs
	- query
		- query string for a CGI program
	- fragment
		- references to a subset of an object 

### URL and HTTP
- all parts of URL, except parameters, used with http
- scheme and host can be omitted when referenced object is on same machine as referring document
- port can be omitted so long as referenced host is running on port listed in your /etc/services file
	- usually on port 80
- full path used when referring to another server
	- relative path on same server
		- reference with relative path is a partial URL
- query passes parameters to CGI
- fragment jumps to labels within a page 
![[Pasted image 20250121092700.png]]
![[Pasted image 20250121093206.png]]

### HTTP Headers
- regardless of the HTTP version the general structure is the same
- requests are sent by the client
- responses are sent by the server
- the client transmits all relevant information in a request header
- the server responds with the requested data and a response header
- the header is dynamic in size depending on the number of fields 

#### HTTP Request Header
- request header fields:
	- accept encoding
	- accept language
	- cookie
	- security
	- user agent 

#### HTTP Request Methods
- request methods are sent in the HTTP request and indicate the desired action for a resource
- most common operations:
	- GET to retrieve a resource
	- POST to submit data
- other common methods:
	- HEADER
	- PUT
	- DELETE
	- OPTIONS 

##### GET
- data send in URL as key/value pairs
- can be cached
- remainds in browser history
- can be bookmarked
- max length of 2048 characters (max URL length)

##### POST
- Data send in request body
- not cached, cannot be bookmarked
- no length restriction

![[Pasted image 20250121094526.png]]![[Pasted image 20250121094558.png]]

### HTTP Response Header
![[Pasted image 20250121094817.png]]
- information about the resource
- can be requested on its own with the HEAD method
- contains the status code, MIME type, and data length
	- 200 = OK
	- 301 = redirect
	- 404 = not found
	- 500 = internal server error
- caching information
- cookies
- encoding (compression)

### HTTP Status Codes
- status codes indicate whether the response has been successfully completed
- Informational responses (100–199)  
- Successful responses (200–299)  
- Redirection messages (300–399)  
- Client error responses (400–499)  
- Server error responses (500–599)

### HTTP Redirects
- status code 301 is used to redirect or forward a client to a new location in case of reorganization of a site
- often used to enforce HTTPS 

### HTTP Cookies
- HTTP cookies are small amounts of data that a server can store in a user’s web browser or client. The browser can then send the cookie alongside a future request
- Primary uses for cookies:  
	- Session Management  
		- Logins, Shopping Carts, Scores  
	- Personalization  
		- Themes, Settings, Preferences  
	- Tracking  
		- Personalized id to track users