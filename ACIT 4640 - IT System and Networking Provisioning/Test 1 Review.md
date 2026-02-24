## Week 1
### basic intro Terraform
- terraform is an Infrastructure as Code (IaC) tool to provision and manage resources in any cloud or data center
- instead of creating EC2 instance in AWS console, you can configure in plain text using Terraform

#### alternatives
- AWS has CloudFormation and CDK as AWS specific solutions
- Pulumi

### Packer
- packer is used to create images that can run in VMs or containers
	- vs terraform that is used to create resources, networking, a database cluster, VM
- can be used to create identical machine images for different platforms from the same packer template

### Ansible
- both ansible and terraform can be used to deploy and manage infrastructure, generally, terraform is used to deploy infrastructure and ansible is used to manage some of that infrastructure 

### important terminology 
- infrastructure
	- infrastructure is the components that make up a system used deploy and maintain applications and services
		- like VM, load balancer, networks
- provisioning
	- process of creating and setting up IT infrastructure and includes the steps required to manage user and system access to resources
	- early stage in the deployment of servers, applications, network components, storage, edge devices,
- infrastructure as code
	- infrastructure management provision using code instead of manually
	- plan text files with instructions to provision and manage infrastructure
	- can be shared with a  team using version control systems like Git
	- benefits:
		- reproducible
		- easier to scale
		- easier to maintain
	- declarative 
		- defines the desired state of the system, including what resources you need and any properties
	- imperative
		- defines specific commands needed to achieve desired configuration and those commands then need to be executed in the right order 
- devops
	- practice of using developer mindset and workflow to perform operations tasks
	- allows ops team to perform tasks faster and more reproducible 


## week 2 - Bash

### SSH review
- create new key pair `ssh-keygen -t ed25519 -f ~/.ssh/keyname -C "comment"`
	- t = type
	- f = filename
	- c = comment 
- connect using your ssh key `ssh -i ~/.ssh/privatekey user@host`
	- i = identity file, your private key
- `~./ssh/config` for config file to configure ssh connections
- `~/.ssh/known_hosts` contains list of known servers that we identified are safe

### Bash scripting review
#### environment variables
- they allow variables to be shared between processes
- `export PASSWORD="good password"`
- export keyword to create a new environment variable 
- tend to be in all caps by convention, not necessity 

#### command substitution
- makes it possible to store the results of a command in a variable
- `today=$(date + '%a'`

#### heredocs
```
cat <<- EOF > file.text
	directory is $PWD
	logged in as $(whoami)
EOF
```
- can also send the contents of the heredoc to the SSH command and allows you to run commands on a server in a script that you run locally
```
ssh -T user@host << EOF
echo "The current local working directory is: $PWD"
echo "The current remote working directory is: \$PWD"
EOF
```

#### linux service review
- a linux service or daemon is a background process that waits for events to occur
- linux service manager is a utility that performs operations like starting, stopping, and monitoring services
	- several service manages
		- dinit
		- openRC
		- systemd
- when talking about managing services in linux using systemd, we talk about several components
	- systemd
	- unit files
	- targets
	- systemctl 
		- utility used to set and view the status of services 
- unit files
	- define a particular service
	- programs that need to be running, including dependences
	- unit files for a service end in `.service`
	- can be found in 
		- `/usr/lib/systemd/system/` units provided by installed packages
		- `/etc/systemd/system` units installed by system admin
- systemd targets
	- a target groups together systemd units through dependencies to create a desired set of units that are running to meet a "target"

#### systemctl utility
- `systemctl enable unit` enables a unit to auto start at boot
- `systemctl status
- `systemctl start`
- `systemctl stop`
- `systemctl restart`
- `systemctl daemon-reload` reload systemd manager configuration2
- `systemctl mask unit` make it impossible to start 
## week 3 - AWS
### what is aws
- AWS CLI is open source tool that enables interacting with AWS services using command line shell
- all IaaS AWS administration, management, and access functions in the AWS management console are available in the AWS API and CLI
- build in help command `aws help`

### configure the AWS CLI
- for general use need the following info
	- access key ID
	- secret access key
	- AWS region
	- output format
- `aws configure` to set up CLI installation
- can import access keys using .csv instead of configure
- `aws configure import --csv file://credentials.csv`

### IAM user
- Identity and Access Management 
- IAM user:
	- lives inside single AWS acc
	- has credentials
	- granted permissions to AWS services

#### what they're used for
- allow humans to sign into AWS Management Console
- allow applications or scripts to access AWS via API/CLI
- enforce least-privilege access

#### directly editing config and credentials
- to directly edit the config and credential files
- `~/.aws/credentials` on linux
- `%USERPROFILE%\.aws\credentials`

## Week 4 - Terraform
### basic intro Terraform
- terraform is an Infrastructure as Code (IaC) tool to provision and manage resources in any cloud or data center
- instead of creating EC2 instance in AWS console, you can configure in plain text using Terraform
- terraform config is declarative
	- you describe what you want to create rather than the steps needed to create that thing

### terraform commands
- `terraform -help`
- `terraform init` prepare the working directroy for other commands
- `terraform validate` check whether config is valid
- `terraform plan` show changes required by current config
- `terraform apply` create or update infrastructure
- `terraform destroy` destroy previously created infrastructure 
- `terraform fmt` reformat your config in standard style

#### order we do it in
- init
- fmt
- validate
- plan
- apply

### writing terraform configuration

```
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "tf-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}
```
- provider block
	- tells terraform which cloud or service to talk to or how
- resource block
	- used for
		- VMs
		- databases
		- buckets
		- networking
		- etc.
- variable block
	- defines inputs to your configuration
	- used for
		- reusability
		- environment specific values
		- avoid hard coding
	- output block
		- exposes values after apply
#### arguments
`instance_type = "t2.micro"`
- arguments are essentially a key value pair

#### blocks
```
resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"
  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```
- a block is comprised of 
	- block type
	- labels

### Terraform state
- stores information about manage resources in a state file `terraform.tfstate` in your project root
- JSON file not intended to be read or edited by hand
