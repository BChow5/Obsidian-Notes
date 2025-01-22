- **Mainframe**: centralized, powerful, and reliable platforms used for large scale data processing, transaction processing, and enterprise applications
- **Client**: device, application, or system sending requests to server to access services or resources.
- **Server**: device, application, or system that provides services, resources, or functionality to clients
- **n-Tier**: A software architectural or design pattern that separates an application into distinct, distributed layers, each with specific responsibilities.
	- 1 tier: all the required components for the working of application are available under the same package.
	- 2 tier: user interface layer and the database layer are located in two different machines
		- client and the server are not on the same computer
	- 3 tier: the presentation (client) layer, the business (application) layer and the database layer are located in separate machines.
	- The difference between the 3 tier and n tier is that there is more than one application server intermediating between the user interface layer and the database layer.
- **Microservices**: cloud based architecture where a single application is composed of many independently deployable smaller components or services 
	- pros: code easily updates, scalable, teams can use different stacks and programming languages for diff components 
	- cons: increased complexity, network latency, security vulnerabilities 
- **Monolithic**: a single codebase executes multiple business functions
	- pros: easier development, simple deployment, increase security
	- cons: reduced scalability, hard to integrate new tech
- **Private cloud**: cloud computing environment that is used exclusively by one organization
- **Public cloud**: cloud computing model where services are provided by a third-party provider and shared across multiple organizations
- **Hybrid cloud**: combination of private and public cloud services, allowing data and applications to be shared between them.
- **Make**: produce or develop a product, service, or system internally within the organization
	- pros: full customization and integration of the system
	- cons: significant investment in resources, time, and expertise, long lead times
- **Buy**: purchase a ready-made product, service, or system from an external provider rather than creating it internally
	- pros: faster implementation, vendor support
	- cons: limited customization, reliance on vendor support and updates
- **Rent**: lease or subscribe to a product, service, or system
	- pros: reduces upfront costs, provides scalability, and offers flexibility
	- cons: ongoing rental or subscription fees, limited control or customization options
- **Configuration**: Uses existing features and settings to adapt to a business' needs. Simpler and less expensive than customization. Doesn't require programming knowledge, and can be done using tools that are already built into the application.
- **Customization**: Making changes to the software's core code to add new features, change its behavior, or integrate it with other applications. Requires specialized development skills and an understanding of the software's architecture. More expensive and time-consuming than configuration.
- **Software-as-a-Service (SaaS)**: software applications that are hosted by a cloud provider and made available to customers over the internet
- **Infrastructure-as-a-Service (IaaS)**: provides virtualized computing resources over the internet, such as virtual machines, storage, and networking
- **Platform-as-a-Service (PaaS)**: provides a platform allowing customers to develop, run, and manage applications without dealing with the underlying infrastructure
- **Shadow IT**: the use of IT systems, devices, applications, or services without explicit approval from the organization's IT department
- **Elastic scaling**: the ability of cloud infrastructure to automatically scale resources up or down based on demand
- **Function (i.e. functional area)**: refers to a specific set of related activities within an organization that are focused on achieving a particular business objective
- **Context diagram**: a high-level visual representation of a system, showing its boundaries and interactions with external entities.
- **Actor** :an entity (person, system, or organization) that interacts with a system. Like a role
- **External Entity**: an actor or system that interacts with a business system but is not part of the system itself
- **Business Process**: a series of tasks that are completed in order to accomplish a goal
- **Business Process Management (BPM)**: discipline that uses various methods to discover, model, analyze, measure, improve and optimize business processes
- **Business Process Model**: visual way (i.e. a picture) to represent how an organization performs work and services
- **Class**: structured way to model real-world entities by defining their common characteristics and behaviors. they have name, attribute, behaviour (methods)
- **Object**: specific instance of a class
- **UML**: Unified Modeling Language provides a standard way to visualize the design of a system.
- **Use case**: story about an actor interacting with a system under specific circumstances to produce an outcome of value (the reason why)
- **Happy Path**: sequence of steps in a use case where everything works as expected without errors or deviations

**Use Case Diagram**
![[Pasted image 20241205233405.png]]


**Fully Dressed Use Case**: Use Case Title, Primary Actor, Pre-condition(s), Success Scenario (or "Happy Path"), Extensions or Alternative Flow(s), Post-condition(s)

**Class Diagram**
![[class diagram2 1.png]]
- arrow points to class they inherit from 

**Swim Lane Diagram/ Cross-functional flowchart**
![[Pasted image 20241209191130.png]]
- circle = start/end, square = task, square with lines on side = has subtasks, diamond = decision