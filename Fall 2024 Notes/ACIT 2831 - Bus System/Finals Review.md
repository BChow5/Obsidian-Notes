- architectures, acquisition/dev approaches, classes and objects, use cases

### Week 9 - System Architecture

#### Overview
- characteristics, benefits, drawbacks of mainframes, client servers and n-Tier architectures
- monolithic and microservices deployments 

#### Review 
- mainframe
	- scalability
	- expensive
	- more secure
	- everything in one
	- not flexible; hard to adopt new systems or tech
- client server
	- client connect to server to get their data
	- flexible and easy to maintain 
	- network dependent 
- n-tier 
	- server is 1 tier
	- client is 2 tier
	- database is another tier
- monolithic
	- everything in 1 file for your system
	- co-dependent 
	- one thing breaking in the system can break everything
	- easier to manage
	- bad scalability 
- microservices 
	- made up of several smaller modules 
	- scalable
	- can be harder to manage 

### Week 10 - Development and Acquisition strategies
#### Overview
- characteristics, benefits, and drawbacks of Make, Buy, Rent approaches 
- Make is the most expensive because we have to pay for everything needed 
	- make is rare 
- Buy and rent and very similar
	- rent is that we don't have to host the stuff ourselves 

#### Review
- make
	- when an org needs custom software for their business
	- pros
		- greater control
		- can integrate to their own system
	- cons
		- expensive
		- takes time to make
- buy
	- standard business model without much customization so you buy
	- can buy it quickly
	- vendor has update
	- can't customize much to own choice
	- vendor dependent 
	- could be challenging to integrate to your system
- rent
	- similar to buy
	- cheaper upfront 
	- scalable
	- updates from vendor
	- can be more expensive in the long run
	- security concerns due to being on cloud
	- limited customization 
- IAAS
	- infrastructure as a service
	- high level of flexibility 
	- storage and database stuff 
- SAAS
	- software as a service 
	- cloud based model
	- subscription model
	- scalable
	- lower initial cost
	- regular updates
- PAAS
	- platform as a service
	- host and manage your applications/site stuff 
	- e.g. google cloud, AWS, Azure 

### Week 11 - Business Process management and model
#### Overview
- definition and benefits of BPM, Actors, swim lane diagrams and shapes

#### Review
- Business Process Management
	- for optimizing business processes
	- the behaviour of system and people
	- can increase efficiency and cost savings
	- enhance employee and customer experience
	- reduce repetitive tasks 
- Business Process Model
	- waterfall type steps or agile model
- swim lane diagram
	- type of flow chart
	- illustrates who is responsible for each step i nthe process
	- describes how the steps relate to different actions and actors
	- shapes represent tasks and handoffs to other team members
- actor
	- person department, or system
	- its a role 
### Week 12 - Object Oriented Analysis and Design (OOAD)
#### Overview
- identifying and defining Classes and Objects, Attributes, and Methods

#### Review
- **class**
	- A class is a blueprint or template for creating objects. 
	- It provides a structured way to model real-world entities by defining their common characteristics and behaviors
	- three things that define a class
		- name
			- The name of the class, representing the concept it models
		- attributes
			- the properties or characteristics of the class. They define the state of an object created from the class.
			- we only include the attributes we need for our system
		- behaviours (methods)
			- the actions or functions the class can perform
- **object**
	- An object is a specific instance of a class
	- a class defines the structure and capabilities of the object, the object itself represents an actual realization of that blueprint.
	- key characteristics
		- Objects have specific values assigned to their attributes
		- Objects are created in memory using the class definition as a guide
- class diagram
	- a class diagram in the Unified Modeling Language (UML) is a type of static structure diagram that describes the structure of a system by showing the system's **classes**, their **attributes**, **operations** (or methods), and the **relationships** among objects.
### Week 13 - Use Cases and Use Case Diagrams
#### Overview
- definition of use cases, use case diagrams, fully dressed use cases

#### Review
- use cases 
	- story that explains who, what, and why
		- who is using the system
		- what actions do they want to do
		- why are they using this system
	- how someone interacts to get a result
- use case diagram
	- shows system, actors, use cases, and connections 
- fully dressed use case
	- all the details about one task
	- has things like title, primary actor, pre condition, happy path, post condition, alternative path