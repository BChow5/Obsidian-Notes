- implementation is  the moment of deployment
	- the moment we make something available in production (live and being used)
	- this is the SDLC step

### Implementation
- once the new system is developed and tested, it needs to be implemented in the organization
- this phase includes training the users, providing documentation, and data conversion from the previous system to the new system
- implementation can take many forms
	- depends on the type of system, number and type of users, and how urgent it is that the system become operational 

#### implementation methodologies
- methodologies 
	- <mark style="background: #ABF7F7A6;">direct cutover</mark>
		- terminate the "old" system and start the new one at a certain date
		- old system completely replaced 
		- use it when  the legacy is so bad or if there is an urgent need
		- the impact of failure in the new system is not very high
	- <mark style="background: #ABF7F7A6;">parallel operation</mark>
		- new and legacy system together simultaneously (at the same time)
		- for how long? it depends
		- most expensive (2x)
		- lowest risk
		- to reduce risk for the new system
		- would do when unsure that the new info system will work right away 
	- <mark style="background: #ABF7F7A6;">pilot operation</mark>
		- new system is introduced to a small number of users before rolling out to everyone
		- want to know what works
		- want user feedback 
		- pilot phase length depends
		- users could be geographically or organizationally isolated 
		- looking for good feedback 
		- lower risk 
		- less expensive than parallel 
	- <mark style="background: #ABF7F7A6;">phased operation</mark>
		- In the **Software Development Life Cycle (SDLC)**, a **phased operation** (also known as a **phased implementation** or **phased rollout**) is an implementation method where a system is deployed incrementally, in stages or phases, rather than all at once. 
		- Allows parts of the system to go live gradually, reducing risk and allowing for more controlled testing and feedback at each stage.
		- can be costly because of the time it takes to go through iterations 
		- length depends for each phase 
- implementation methodologies are IN the implementation step
	- they are part of, not a replacement, for the SDLC 

#### Implementation tasks
- data conversion
	- ensuring that data in the old system can work with the new system when being migrated over 
- training
	- giving users the skills to use the new technology
	- could train people who will have to train others how to use it
	- could be documentation on how to use the new tech 
- documentation
	- document what is different between the old and new systems
	- document how to do things 
	- might be documenting how jobs will change from new tech
	- info for troubleshooting help 
- maintenance 

#### Testing
- unit testing
- system testing
- User Acceptance Testing (UAT)