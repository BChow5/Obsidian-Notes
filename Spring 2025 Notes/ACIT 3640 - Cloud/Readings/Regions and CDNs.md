### Virtual Machines
- virtual machines are where we run our software or code
	- e.g. EC2 instances, lambda functions, containers

### Servers
- the virtual machines run on a piece of hardware: the compute server
- when we rent some compute in the cloud, we are getting a VM inside a larger compute

### Data centers 
- warehouses full of hardware

### Availability Zones 
- an Availability Zone (AZ) is some number of data centers, all located close to each other
- we can think of an AZ as a single unit
	- a "logical data center" made up of many physical data centers 
![[Pasted image 20250117134842.png]]

### Regions
- region is  a place in the world like `us-east-1` 
- regions contain at least two Availability Zones for redundancy 
- AZ are physically separated by a meaningful distance from each other
- they are linked to each other with a low latency, high bandwidth, private network
![[Pasted image 20250117135119.png]]
- We have to chose an AZ and region to host most things in the cloud. 
- We can host things in more than one region, but that increases the cost of hosting.

#### Choosing a region

1. Compliance with data governance and legal requirements: data never leaves a region without your explicit permission
2. Proximity to customers: reduced latency
3. Available services within a Region: new services and new features aren't available in every Region
4. Pricing: pricing varies region to region and is transparent in the service pricing page

### S3 and CloudFront
- These services work together to store your data and deliver it quickly to users around the globe

#### Amazon S3: Simple Storage Service
- like a hard drive that never runs out of space
- S3 is a storage unit that expands automatically when you need more space
	- cheap and can use to host a static website
- ONLY static files 
	- If you need to actually run some server-side logic, you'll need to use a compute service like EC2 or Lambda

It works like this:
1. You upload your files (anything from cat pictures to complex application data).
2. AWS stores these files within a single region you choose.
3. You can retrieve, delete, or update these files whenever you need.

#### CloundFront: Your Global Content Delivery Network
- CloudFront is AWS's content delivery network (CDN)
- it takes the files you've stored in S3 and distributes them to edge locations all around the world
- these edge locations are like _mini_ data centers strategically placed to be as close to end-users as possible
- CloudFront doesn't just work with S3, though. 
	- It can deliver content from pretty much anywhere, but it's most commonly used to deliver content from S3.
- Just remember that you don't store files in CloudFront, it's a CDN not a storage service
- So you store your files in an S3 bucket, then CloudFront handles caching and delivery at the edge.

**Here's how it works:**
- A user in Tokyo tries to access your files hosted in a US-based S3 bucket.
- Instead of fetching the data all the way from the US, CloudFront serves it from an edge location in Asia.
- The user gets the content faster, and everyone's happy.

#### Regions and Edge Locations: The global infrastructure 
- When you use S3, you choose a specific region to store your data
	- Your data stays in that region unless you explicitly move it
- CloudFront, on the other hand, uses a global network of edge locations.
- When a user requests content, CloudFront routes the request to the edge location that can best serve the request based on network conditions and location
- This combination of regional storage and global delivery allows you to optimize for both data sovereignty (keeping data in specific geographic areas) and performance (delivering content quickly to users worldwide).





