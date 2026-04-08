### Definitions
- enterprise
	- a company or a business
- what is the difference between cloud and on premise?
	- cloud
		- severs, storage, applications are hosted by third party provider over the internet
	- on premise
		- company owns and operates it's own physical servers and infrastructure inside its building or private data centers
	- when do you do on prem vs cloud?
		- on prem
			- strict regulatory or data requirements
			- extremely sensitive data
			- stable and predictable workloads
			- already have large infrastructure invested
		- cloud
			- needing scalability or unpredictable growth
				- e.g. start ups, seasonal, rapid expansion
			- lower upfront costs
			- don't want to manage infrastructure
			- building cloud apps
- what is the difference between IaaS and SaaS?
	- IaaS Infrastructure as a Service
		- cloud provider gives
			- VMs, storage, networking, basic infrastructure
		- why choose it?
			- need high customization
			- want control over OS and config
			- DevOps
	- SaaS Software as a Service
		- complete application ready to use
		- provides
			- infrastructure, OS, updates, security patches, application itself
		- why choose it?
			- fast deployment
			- don't want to manage
			- don't need deep customization
			- standard software

### requirements
- what is a functional requirement?
	- describes what a system must do
	- specific behaviours, features, or functions it must provide
	- e.g. be able to create an account, be able to log in or out
- what is a non functional requirement?
	- describes how well a system must perform it's functions
	- not what it does, but the quality attributes and constraints around it
	- performance, reliability, security, user experience
	- e.g. page must load in under 2 seconds
- what's the difference between them?
	- functional describes what the system does
		- they define features, user interactions, system behaviors
	- non functional describes how the system performs those functions
		- performance, security, reliability, usability, scalability, compliance

### stakeholders
- what is a stakeholder?
	- individuals or groups who have interest in, influence, or affected by project, system, decision
- who would be examples in IT project?
	- users
	- project managers 
	- developers 
	- IT team

### tool assessments
- tool assessment will vary based on who is looking at the tool and for what purpose
- team managers
	- collab and knowledge sharing
	- knowledge organization
	- access control
	- reporting
	- care about visibility, productivity, governance, ROI
- software developer
	- ease of use
	- developer friendly
		- good documentation
	- integration
	- performance
	- migration support
- IT department
	- security
	- infrastructure requirements
	- maintenance and operations 
	- licensing and cost

### devops
- what are the three primary practice areas of dev ops?
	- provisioning
		- setting up and managing infrastructure
		- e.g. creating servers, config networks
	- continuous integration and delivery
		- automating the software build, test, and deployment process
		- e.g. gitlab, jenkins, github
	- reliability
		- keeping systems stable, available, and performant
		- e.g. disaster recovery, incident response, logging
- what does a continuous integration pipeline do?
	- automated workflow that ensures code changes are integrated, tested, and verified continuously before merged into the main codebase 
- what are characteristics of a good CI pipeline
	- fast feedback
	- reliable and deterministic
	- automated
	- version controlled
	- scalable
- what types of files should go in your source code repository? what shouldn't?
	- should:
		- source code
		- config files
		- scripts
		- documentation
		- unit tests
	- shouldn't:
		- dependencies/libraries
		- secrets or sensitive info
		- generated/build artifacts
		- large binary files
		- temp or log files

### pipelines - jenkinsfile
- what are the main keywords we have used in jenkinsfile? what do they do?
	- pipeline
		- declares the start of a pipeline script
		- everything else is nested inside this block
		- like containers 
	- agent
		- specifies where the pipeline or stage will run
		- common values
			- any - run on any available agent/node
			- none - no global agent, stages define their own
			- label 'linux' - run on agent with label linux
		- ensures your builds run on appropriate machine/environment
	- stages
		- groups multiple stages of the pipeline
		- helps to organize the pipeline into logical steps
	- stage
		- define a single step or phase in your pipeline
		- e.g. build, test, deploy
		- each stage can have its own steps, agent, or environment
	- steps
		- contains actual commands to execute in a stage
		- can include:
			- sh - run shell commands
			- bat - run batch commands
			- echo - print messages
	- environment
		- define environment variables for the pipeline or a stage
		- useful for credentials, paths, or build configurations 
	- post
		- actions that run after a stage or pipeline completes
		- can handle different outcomes:
			- always - always run
			- success - only if successful
			- failure - only if failed 
	- script
		- run arbitrary groovy code inside declarative pipelines
		- needed if logic cannot be expressed declaratively 

#### Examples

**Pipeline**
```groovy
pipeline {
    agent any
    stages { ... }
}
```

**agent**
```groovy
agent any
```

**stages**
```groovy
stages {
    stage('Build') { ... }
    stage('Test') { ... }
}
```

**stage**
```groovy
stage('Build') {
    steps {
        echo 'Building...'
    }
}
```

**steps**
```groovy
steps {
    sh 'mvn clean package'
}
```

**environment**
```groovy
environment {
    JAVA_HOME = '/usr/lib/jvm/java-11'
    PATH = "$JAVA_HOME/bin:$PATH"
}
```

**post**
```groovy
post {
    always { echo 'Cleaning up...' }
    failure { mail to: 'devteam@example.com', subject: 'Build Failed' }
}
```

**script (optional)**
```groovy
script {
    def version = sh(script: 'git describe --tags', returnStdout: true).trim()
}
```

#### shared library
- how is a shared library defined?
	- a shared library is a way to re-use pipeline code across multiple Jenkinsfiles
	- a share library is essentially a shared git repository that contains reuseable pipeline
	- jenkins can load it into any pipeline using the @library annotation
	- separate repo - CI functions
	- Jenkins management to make it available and trusted
- what is the advantage of a shared library?
	- can be reused
		- DRY principle - don't repeat yourself
	- centralized maintenance
	- consistency and standardization

**directory structure**
typical shared library has this structure
```groovy
(root of repo)
├── vars/
│   └── myFunction.groovy      # Global functions accessible in pipelines
├── src/
│   └── org/example/
│       └── Helper.groovy      # Groovy classes for more complex logic
├── resources/
│   └── myTemplate.txt         # Non-Groovy files, templates, configs
└── README.md
```
- - `vars/` → contains scripts that can be called directly in a Jenkinsfile as functions or steps.
- `src/` → contains Groovy classes (namespaced) for more structured code.
- `resources/` → static files, templates, or configs that your pipeline might use.

**defining a function in `vars/`**
e.g. `vars/notifyTeam.groovy`
```groovy
def call(String message) {
    echo "Notifying team: ${message}"
    // Add actual notification logic here
}
```
- the function name matches the file name
- you can call this directly in a Jenkinsfile

```groovy
@Library('my-shared-library') _
pipeline {
    agent any
    stages {
        stage('Notify') {
            steps {
                notifyTeam('Build started!')
            }
        }
    }
}
```

**using classes in `src/`**
- e.g. `src/org/examples/Helper.groovy`
```groovy
package org.example

class Helper {
    static void sayHello(String name) {
        println "Hello, ${name}"
    }
}
```

**loading the shared library in Jenkins**
- Go to **Jenkins → Manage Jenkins → Configure System → Global Pipeline Libraries**.
- Add a library:
    - Name: `my-shared-library`
    - Source Code Management: Git repository URL
    - Default version: branch or tag (e.g., `main`)
- In your pipeline, reference it with:
```groovy
@Library('my-shared-library') _
```

#### pipelines
- where do we store our pipeline definition? 
	- pipelines to automate, standardize, and safeguard the process of building, testing, and delivering software
	- core purpose of pipeline
		- code -> build -> test -> validate -> deploy
	- pipeline definition is stored in source control
	- inside the project repository in a file called `Jenkinsfile`
	- this approach is called Pipeline as Code 
- how often should we run the pipeline?
	- run every time code changes
	- goes in the repo of the code it's for 
- why do we have pipelines?
	- for code testing
	- automation
	- makes developers push code frequently to get them integrated together
	- early problem detection
	- consistency

### software updates
- what is the production environment for a software installation?
	- live environment where the software runs for real users
	- environment used by users
- how often do you update the production environment?
	- depends
		- determined by:
			- qualtiy of testing
			- deployment automation
			- business needs
	- should have some sort of schedule
	- older or high regulated:
		- every few months or quarterly
	- modern devops:
		- weekly, daily, multiple times a day
- what are the planning activities before an update?
	- define scope of update
	- backup
	- test plan
	- rollback plan
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
