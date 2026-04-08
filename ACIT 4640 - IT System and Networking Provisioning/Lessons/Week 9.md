quiz
- name three standard files commonly found in a terraform module and describe their purpose
	- main.tf
	- input variable
	- output variable
	- readme
- what is purpose of the source and version arguments in terraform module block
	- source - where to fetch module from
	- version - which version of the module 
- difference between root and shared module
	- root is the module we do our terraform init
	- shared module is modules that are like plugins or packages. additional source code we can share
		- e.g. pandas
	- main purpose of input variables in terraform module?
		- like function arguments in python 
- what are input variables, output variables, and local variables if you were doing an analogy comparing them to parts of a python function? 
	- input - arguments/parameters 
	- output - return value
	- local - variables 
- T or F terraform variables are constant?
	- True, terraform is declarative
- T or F terraform has multiple number types like whole, fractions, negative
	- False

### terraform variables

#### terraform input variables
- input variables are parameters that allow users to send info into a module
- they make module reusable and customizable without changing their source code

**key points**
- defined using the `variable` block
- accessed inside a module using the `var <variable name>` syntax
- can include descriptions, type constraints, default values, and sensitive flags 
- analogy: like function arguments in a programming language such as python
	- they provide data to the module

**example**
```
variable "instance_type" {
  type        = string
  description = "The type of EC2 instance to launch"
  default     = "t3.micro"
}
```

#### terraform output variables
- output variables are used to return information from a module to its caller (another module or the root configuration)

**key points**
- defined using `output` block
- typically used to share resource attributes (like instance ID or IP addresses)
- can also be marked as `sensitive` to hide values in logs
- like function's return value. they send data out of the module 

**example**
```
output "instance_ip" {
  description = "Private IP address of the instance"
  value       = aws_instance.example.private_ip
}
```

#### terraform local variables
- Local variables are values that **exist only inside a module**, used for intermediate calculations or to simplify repeated expressions.

**key points**
- Defined inside one or more `locals {}` blocks.
- Accessed using the `local.<variable_name>` syntax.
- Can reference other locals or inputs but cannot be accessed outside the module.
- Analogy: Like **internal variables** within a function — used for internal logic and not exposed externally.

**example**
```
locals {
  common_tags = {
    ManagedBy = "Terraform"
    Environment = var.environment
  }
}
```

![[Pasted image 20260303090143.png]]

### terraform modules
Technically everything in Terraform is a module, we have been writing modules all along.

Every Terraform project has at least one module, the root module.

A module that has been called by another module is often referred to as a _child module._

Modules allow you to write reusable code that you can store locally or remotely.

Modules can be used to create infrastructure, or deployment stages (testing, production), or for a host of other scenarios where it is handy to have separate, re-usable configuration.

A module contains Terraform configuration. So you might have a main.tf, variables.tf and outputs.tf in a module.

A terraform Module will typically contain more variables, so that it is more flexible, and can be used to solve a similar set of problems.

### Where can modules can be stored[](#where-can-modules-can-be-stored)

- Locally in a directory on your machine
- Modules can be stored on the Terraform Registry and the Terraform cloud (Now Hashicorp Cloud Platform)
- Remote Git (or other version control) repository
- In an S3 bucket
- You can host your own at an HTTP end point

### What is the difference between resources and modules in Terraform?[](#what-is-the-difference-between-resources-and-modules-in-terraform)

> A resource in Terraform describes a piece of infrastructure that is going to be created (e.g., a VPC, a subnet, an ec2 instance, etc), whereas a module is a collection of resources that are used together to achieve a reusable use case.

## Building a module[](#building-a-module)

> A _Terraform module_ is a collection of standard configuration files in a dedicated directory. Terraform modules encapsulate groups of resources dedicated to one task, reducing the amount of code you have to develop for similar infrastructure components.
> 
> - [Spacelift](https://spacelift.io/blog/what-are-terraform-modules-and-how-do-they-work)

The recommended structure of a module looks like this:

You have a `modules` directory that contains directories for your individual modules. Each module includes a `main.tf`, `outputs.tf` and `variables.tf` file that make up that module.

```
.
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
├── ...
├── modules/
│   ├── nestedA/
│   │   ├── README.md
│   │   ├── variables.tf
│   │   ├── main.tf
│   │   ├── outputs.tf
│   ├── nestedB/
│   ├── .../
```

## Using a Terraform module[](#using-a-terraform-module)

You reference a module in a module block.

```
module "terraform_test_module" {
 source  = "./module/terraform-test-module"
 version = "1.0.0"
 
 argument_1                     = var.test_1
 argument_2                     = var.test_2
 argument_3                     = var.test_3
}
```

### passing data into your module with variables[](#passing-data-into-your-module-with-variables)

Inside of your modules variables file

```
variable "instance_type" {
  description = "The type of EC2 Instances to run (e.g. t2.micro)"
  type        = string
}
```

inside of your modules main file

```
resource "aws_launch_configuration" "example" {
  image_id        = "ami-0fb653ca2d3203ac1"
  instance_type   = var.instance_type
  security_groups = [aws_security_group.instance.id]

  user_data = templatefile("user-data.sh", {
    server_port = var.server_port
    db_address  = data.terraform_remote_state.db.outputs.address
    db_port     = data.terraform_remote_state.db.outputs.port
  })

  # Required when using a launch configuration with an auto scaling group.
  lifecycle {
    create_before_destroy = true
  }
}
```

In your root main file

```
module "webserver_cluster" {
  source = "../../../modules/services/webserver-cluster"

  cluster_name           = "webservers-stage"
  db_remote_state_bucket = "(YOUR_BUCKET_NAME)"
  db_remote_state_key    = "stage/data-stores/mysql/terraform.tfstate"

  instance_type = "t2.micro"
  min_size      = 2
  max_size      = 2
}
```

### Accessing data in a module[](#accessing-data-in-a-module)

Inside your module code use output variables

```
output "asg_name" {
  value       = aws_autoscaling_group.example.name
  description = "The name of the Auto Scaling Group"
}
```

From your root module you can access module output like so:

```
module.frontend.asg_name
```

## Terraform modules best practices[](#terraform-modules-best-practices)

There are a couple of things you should consider when using Terraform modules:

1. Each Terraform module should live in its own repository, and versioning should be leveraged.
2. Minimal structure: main.tf, variables.tf, outputs.tf.
3. Each Terraform module should have examples inside of them.
4. Use input and output variables (outputs can be accessed with `module.module_name.output_name`).
5. Used for multiple resources, a single resource module is usually a bad practice.
6. Use defaults or optionals depending on the data type of your variables.
7. Use [dynamic blocks](https://spacelift.io/blog/terraform-dynamic-blocks).
8. Use ternary operators and take advantage of Terraform built-in functions.
9. Test your modules.