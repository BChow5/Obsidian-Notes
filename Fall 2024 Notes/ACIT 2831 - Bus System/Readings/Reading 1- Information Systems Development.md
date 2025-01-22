#### Programming
-  software is created via programming
- programming is the process of creating a set of logical instructions for a digital device to follow using programming language 
- the creation of software is resource intensive process that involves several different groups of people in an organization 
- to be effective, groups a free to follow a specific software development methodology for software development 
![[Pasted image 20240911182001.png]]
- Methodologies
	- SDLC
	- RAD
	- Agile
	- Lean 

#### Systems Development Life Cycle
- <mark style="background: #ABF7F7A6;">Systems Development Life Cycle (SDLC)</mark> was developed to manage large software projects associated with corporate systems running on mainframes
- very structured and risk averse
- designed to manage large projects that include multiple programmers and systems that have a large impact on the organization 
- similar to an assembly line process 

##### SDLC Phases
- **Preliminary Analysis**
	- the request for a new system is reviewed to determine the problem, feasibility, and alternative
	- a feasibility  study asses technical, economic, and legal aspects to decide if it should proceed 
- **System Analysis**
	- System analyists work with stakeholders to define specific system requirements without programming
	- involves documenting procedures, interviewing users, developing data requirements
		- results in a system **requirements document** 
- **System Design**
	- a designer creates technical details based on the requirements document
	- this phase translates business needs into technical specifications for user interfaces, databases, and reporting
	- results in a **system design document**  
- **Programming**
	- programmers write code according to the design document to develop initial software program
	- result is a working program that aligns with the specified requirements 
- **Testing**
	- software undergoes unit testing, system testing, and user acceptance resting to identify and fix errors
	- process ensures that the software functions correctly and meets user standards 
- **Implementation**
	- the new system is rolled out
	- including user training, documentation, and data migration
	- approach varies based on system type and urgency
- **Maintenance**
	- post implementation
	- system is supported through bug fixes, feature updates, and backups
	- typically handled as an ongoing operating expense separate from the initial development cost 
##### SDLC as the Waterfall Method
![[Pasted image 20240911183427.png]]

- The SDLC methodology is sometimes referred to as the waterfall methodology to represent how each step is a separate part of the process
- Only when one step is completed can another step begin
- This methodology has been criticized for being quite rigid, allowing movement in only one direction, namely, forward in the cycle
	- e.g. changes to the requirements are not allowed once the process has begun. No software is available until after the programming phase.
- Because of its inflexibility and the availability of new programming techniques and tools, many other software development methodologies have been developed.

#### Rapid Application Development

![[Pasted image 20240911183734.png]]

-  <mark style="background: #ABF7F7A6;">Rapid Application Development (RAD)</mark> focuses quickly building a working model of software, getting feedback from users, and then using that feedback to update the working model
	- after several iterations of development, a final version is developed and implemented 

##### RAD methodology consists of 4 phases
1. **Requirements planning**
	- This phase is similar to the preliminary analysis, system analysis, and design phase of the SDLC
	- in this phase, overall requirements for the system are defined, a team identified, and feasibility is determined 
2. **User Design**
	- representatives of the users work with the system analysts, designers, and programmers to interactively create the design of the system
	- sometimes a **Joint Application Development (JAD)** session is used to facilitate working with various stakeholders 
3. **Construction**
	- Application developers, working with users, build the next version of the system through an interactive process 
	- changes can be made as developers work on the program
	- this step is executed in parallel with the User Design step in an iterative fashion
		- making modifications until an acceptable version of the product is developed 
4. **Cutover**
	- organization transitions from an old system to a new one
	- timing is crucial, often scheduled during low activity periods
	- **Direct Cutover**: Switching directly from the old system to the new one.
	- **Incremental Cutover**: Introducing the new system in stages, such as by implementing one module at a time.
	- **Parallel Running**: Operating both systems simultaneously to ensure the new system's accuracy and reliability.

#### Agile Methodologies 
- Agile methodologies are a group of methodologies that utilize incremental changes with a focus on quality and attention to detail
- While considered a separate methodology from RAD, the two methodologies share some of the same principles such as iterative development, user interaction, and flexibility to change.
  
##### Agile and Iterative Development
![[Pasted image 20240911184919.png]]
- iterations is the center of agile development
- the above diagram showed how the system is built incrementally, with each block of the project being developed, reviewed, and adjusted based on feedback
- blocks that are not acceptable are revised accordingly
- Daily Review highlights the ongoing evaluation and collaboration between developers and customers to ensure continuous improvement and alignment with project goals

##### Characteristics of agile methodology
- Small cross functional teams that include development team members and users
- daily status meetings to discuss the current state of the project
- short time frame increments (from days to one or two weeks) for each change to be completed
- working project at the end of each iteration which demonstrates progress to stakeholders 

The goal of agile methodologies is to provide the flexibility of an iterative approach while ensuring a quality product 

#### Lean Methodology 

![[Pasted image 20240911185603.png]]

- Lean focuses on taking an initial idea and developing a <mark style="background: #ABF7F7A6;">Minimum Viable Product (MVP)</mark>
- the MVP is a working software application with just enough functionality to demonstrate the idea behind the project
- once the MVP is made, the development team gives it to potential users for review
- feedback on MVP is generated in two forms
	- 1. direct observation and discussion with the users
	- 2. usage statistics gathered from the software itself
- using the feedback, the team determines whether they should continue the same or rethink the core idea of project, change the functions, and create a new MVP
	- the change in strategy is called a pivot
- Several iterations of the MVP are developed, with new functions added each time based on the feedback, until a final product is completed

##### Differences for iterative and non-iterative methodologies
- The biggest difference between the iterative and non-iterative methodologies is that the full set of requirements for the system are not known when the project is launched.
- As each iteration of the project is released, the statistics and feedback gathered are used to determine the requirements
- The lean methodology works best in an entrepreneurial environment where a company is interested in determining if their idea for a program is worth developing