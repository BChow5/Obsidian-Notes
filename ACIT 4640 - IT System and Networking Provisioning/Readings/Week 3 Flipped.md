### What is IaC?
- infrastructure as code
- code used to provision resources
	- VM
	- network infrastructure
	- security groups
	- users
- able to model our infrastructure with code
	- allows for consistency and reliability across tech and architecture
	- ability to repeat our work
- main philosophy of IaC is handled as a set of human readable configuration files 

#### Some core principles
- versioned infrastructure
	- multiple versions of code that should be under source control like Git
- idempotence
	- consistency no matter how many times its run
	- should have  the same well defined state
- self describing infrastructure
	- the infrastructure is the code and can be understood by people easily
- easily re-creatable systems, repeatable processes, disposable systems, design is always changing, generic modules 

### what is Terraform
- Terraform is HashiCorp's IaC tool
	- uses HCL
		- Hashicorp Configuration Language
- locally we use Terraform CLI
	- can run commands like Terraform init, Terraform plan, and Terraform apply which affect the providers that you work with
- providers are like AWS, Azure, Google, Compute, VMware, etc
	- providers are services or systems that Terraform interacts with to build infrastructure 
- it works with their API so Terraform lets you define resources and infrastructure 
- Terraform is declarative
	- configuration files are not explicit
- manages your infrastructure entire life cycle
- meant to be used if you build and take down infrastructure often
	- e.g. development and testing purposes 
	- not meant for one off situations 

### Why use Terraform?
- replicate and modify infrastructure quickly and efficiently
- manage infrastructure on multiple cloud platforms
- human readable configuration language
- easy code to write
	- block types, identities, and expressions are well documented 
- HCL does not support user defined functions
- Terraform state allows you to track resource changes throughout your deployments
	- it remembers dependences between resources
	- we use state so we have consistent runs and it is stored as a state file
	- `terraform.tfstate` or `terraform.tfstate.d/`
- version control
	- you can commit your configurations to version control

### Terraform Help System
- `terraform -help`
- `terraform -h`
	- `terraform -h init`
- tells you the usage and options

### How terraform works
- write your code
	- initialize your directory
		- apply your infrastructure
		- `terraform init`
		- `terraform apply`
- this is known as a terraform run
- `terraform init` initialized the directory and allows us talk to the API
- `terraform plan`
- `terraform destroy`
- `terraform fmt`
- `terraform validate`