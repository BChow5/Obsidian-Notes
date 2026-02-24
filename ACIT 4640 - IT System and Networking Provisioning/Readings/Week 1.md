### Terraform and OpenTofu
- Terraform is an IaC tool used to provision and manage resources in any cloud or data center
- instead of making an EC2 instance in AWS, we can configure it in plain text using Terraform
	- can be shared using Git
- OpenTofu is a fork of Terraform

#### Alternatives
- CloudFormation and CDK are AWS specific solutions

#### Packer
- Terraform is generally used to create resources, networking, a database cluster, VM, etc. 
- Packer is used to create images that can run in VMs or containers
- can be used to create "identical" machine images for different platforms from the same packer template
- after making the image, you would use Terraform to deploy the image to a VM

#### Ansible
- both Terraform and Ansible can be used to deploy and manage infrastructure
	- usually Terraform is used to deploy and Ansible i used to manage some of the infrastructure
- could be used to install software or copy files to a server

### Terminology
- Infrastructure
	- components that make up a system used to deploy and maintain applications and services
		- VM
		- Load Balancers
		- Networks 
- Provisioning
	- process of creating and setting up IT infrastructure
	- includes steps required to manage user and system access to resources
	- provisioning is an early stage in the deployment of servers, applications, network components, storage, etc.
- Management
	- configuration management is a process for maintaining computer systems, servers, applications, and network devices
	- ensures a system performs as expected after changes made over time
- Infrastructure as Code (IaC)
	- infrastructure management provision using code instead of a manual process
	- plain text files contain instructions to provision and manage infrastructure that can be shared with a team using version control systems
	- benefits:
		- reproducible
		- easy to scale
		- easy to maintain 
	- two approaches
		- declarative
			- defines the desired state of the system, including what resources you need, and any properties they should have and the IaC tool will configure for you
		- imperative
			- defines the specific commands needed to achieve the desired configuration, and those commands then needed to be executed in the correct order

### Dev Ops
- practice of using a developer mindset and developer workflow to perform operations tasks
- allows an operations team to perform tasks faster and to make those tasks more reproducible
- 