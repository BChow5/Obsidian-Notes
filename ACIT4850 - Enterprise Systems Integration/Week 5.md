### definitions
- jenkins pipeline
	- definition of a CI/CD pipeline in jenkins
- continuous integration pipeline
	- automated expression of your process for getting software from source code management through to build, test, and packing steps 
- continuous delivery pipeline
	- extension of the CI pipeline to include automated deployment to development and test environments

### jenkins file
- definition of jenkins pipeline
- uses a Domain Specific Language and Declarative syntax based on Groovy language syntax
- should be committed to the repository of the code it builds
- benefits
	- creates a pipeline automated in a jenkins job
	- you can review and collab on the pipeline when checked into source code management
	- audit trail
	- source of truth for the pipeline

![[Pasted image 20260203140118.png]]

### advantages
- code - pipelines are implemented in code. makes it easier to review and collab on the pipeline
- durable - can survive when the jenkins server goes down. they are "backed up" with the source code
- modular - can reuse the same patterns in multiple pipelines

### jenkinsfile
- pipeline - overall pipe definition
- agent - specification for running the pipeline
- stages - one or more stages in the pipeline
- stage - specific state in the pipeline
- steps - one or more actions to perform in the stage 
![[Pasted image 20260203140606.png]]
![[Pasted image 20260203140611.png]]

### jenkins workspace
- each jenkins job has a workspace
- workspace is where files are stored such as
	- files from your Git
	- files generated from your pipeline (e.g. test results)
- the workspace is persistent across builds
	- doesn't automatically get cleared out
- will cause some problems but we address those in week 6
	- test results will accumulate
	- causes our test result graph to show more tests each run
	- may cause an unstable build after correcting a build failure
- most newer CI/CD tools automatically clean out their Workspace of create a fresh Docker container each time the pipeline is run
- 