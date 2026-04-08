## Test Overview
### general
- enterprise development environment
	- what is it and what parts are covered
- tools
	- what does each do?
- concepts - devops
- jenkins/shared library
	- what are the parts of jenkins file and why is it good
	- what is shared library and why is it good
	- jenkins vs gitlab ci
- jenkins vs gitlab CI
	- use cases, similarities and differences
	- architecture 
- on prem vs cloud
	- what are they and when to use
- development workflows
	- trunk based vs feature branching
	- merge/pull requsts, forking, gitflow

### tool assessment
- for given scenario, what would be important to include in an assessment (functional/non requirements)

### improvements
- for given non functional category, what improvements would you make to our enterprise development environment
- e.g. containerization, provisioning, security 

### practical
- jenkins or gitlab CI - create a simple multi stage pipeline for a given python or java code repository

## Review Content

### stakeholders
- consider what is important to following stakeholders when making a product selection for enterprise development environment
	- software developer
		- familiarity
		- features
		- usability
	- project manager
		- cost
		- documentation
		- doesn't slowdown workflow
	- team lead
		- costs
		- administration tools like setting up and managing users
		- usability
		- training 
	- IT operations
		- customer support 
		- backups 
		- ease of installation/updates 
		- security 
		- logging

### enterprise
- what is an enterprise system?
	- system used in an enterprise to support their business operations
		- could be HR or payroll related, product related
	- our development tools in our context
- what is system enterprise integration
	- how different programs or software work together 
	- communication between software
	- e.g. webhooks or pushes on gitlab trigger jenkins
- what is the relationship between enterprise system integration and enterprise development environment 
- why do it? why would a company want an enterprise development environment
	- efficiency and can find bugs earlier while its easier and cheaper to fix
	- consistency for deployment
	- cost savings of some kind
	- could deploy quicker

### improvements
- what improvements would you make to our environment with regards to:
	- security
		- SSO
		- behind VPN
		- all tools updated 
	- usability
		- improved documentation or FAQ
		- SSO
		- consistent URLs
	- upgrade and maintenance
		- backups
		- upgrade regularly 
	- CI/CD infrastructure 
		- runners on separate VM
		- more shared libraries
		- provision new VM and deploy there
			- using things like ansible

### jenkins vs gitlab CI
- what are some similarities and 
	- different language (groovy vs bash/shell script)
	- jenkins doesn't clean up anything 
	- gitlab cleans up after each job
	- jenkins more customizable with plugins 
- which do you prefer?
	- smaller teams gitlab good
		- all in one
	- 
- when to use one or the other?
- what are components of jenkins and gitlab CI configuration? how do they communicate and authenticate
	- jenkins
		- apache -> controller node -> java and python agents
	- gitlab CI
		- gitlab server -> shell runner and docker runner

### CI/CD pipelines
- what is purpose of CI pipeline
	- what do software developers, testers, and company get out of it\
	- input - source code
	- output - artifact
- what is purpose of CD pipeline?
	- input - artifact
	- output - deployment in an environment
- what's the difference between continuous delivery and continuous deployment 
	- delivery deploys to an internal environment
	- deployment has developer changes goes to production at the end 
		- potentially without human interaction 