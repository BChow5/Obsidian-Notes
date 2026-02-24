- what's an enterprise
	- company or business
- whats difference between cloud and on premise? in what cases would you favour one over the other
	- on prem is when company owns and manages physical. cloud is paying third party to rent
	- on prem might be better if you need more control 
	- when is on prem cheaper?
		- continuous services 
	- when is cloud better?
		- good for quick scalability
		- start ups when you're lacking upfront cost for infrastructure
- what is diff between IaaS and SaaS? in what case would you favour one over the other?
	- infrastructure as a service
		- benefit
	- software as a service 
		- benefit
			- less management

### requirements
- what is a functional requirement?
	- something that it needs to do (features)
- what is a non functional requirement?
	- how it needs to do something/constraints 
- whats the difference between them?

### stakeholders
- what is a stakeholder in a software or IT project?
- who would be examples?
	- users
	- project managers 
	- developers 
	- IT team

### tool assessments

### devops
- what are the three primary practice areas of dev ops?
	- provisioning
	- continuous integration and delivery
	- reliability
- what does a continuous integration pipeline do?
- what are characteristics of a good CI pipeline
	- should happen every change
	- useful feedback 
- what types of files should go in your source code repository? what shouldnt?
	- source code
	- terraform and ansible scripts
	- db creation scripts
	- dont put:
		- passwords
		- OS
		- artifacts 

### pipelines - jenkinsfile
- what are the main keywords we have used in jenkinsfile? what do they do?
	- pipeline
	- agent
	- stages
		- stage blocks
			- steps
				- scripts, ssh, code
- how is a shared library defined?
	- seperate repo - CI functions
	- jenkins management to make it available and trusted
	- 
- what is the advantage of a shared library?
	- can be reused
- where do we store our pipeline definition? how often should we run the pipeline?
	- run every time code changes
	- goes in the repo of the code it's for 
- why do we have pipelines?
	- for code testing
	- automation
	- makes developers push code frequently to get them integrated together

### software updates
- what is the production environment for a software installation?
	- environment used by users
- how often do you update the production environment?
	- depends
	- should have some sort of schedule
- what are the planning activities before an update?
	- backup
	- test plan
- what do you do if something goes wrong during update?
	- rollback using backups
- when do you do the update?
	- maintenance window (low usage time)

### enterprise development environment improvements
- if we plan to roll out our enterprise development environment into production, what improvements would we want to do from the perspective of
	- security
		- VPN requirements
		- only ports and protocols we need are on
		- SSO
	- maintainability
		- documentation
		- IaC using terraform and ansible 
		- backups 
	- usability
		- documentation for development teams
		- all apps through apache for consistent URL
		- 
