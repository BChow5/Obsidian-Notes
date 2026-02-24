
### Packer what and why[](#packer-what-and-why)
- Packer is a tool for creating machine images for multiple platforms from a single template. 
- This allows you build images for multiple platforms (different cloud service providers, your on-premise infrastructure, container run-times... ). 
- You can build these images from a single source configuration template and provisioner files that can be managed with version control tools like Git.
- Packer doesn't replace tools like Ansible. Ansible can be used with Packer to build images. You can also use shell scripts and a host of other tools.
- Helps to keep your development, staging and production as close as possible.
- Helps to reduce drift by creating infrastructure that is closer to immutable.

### terminology
- **Image or Machine Image** A pre-configured operating system and installed software which can be run in a container or VM and can have additional configuration added to it.
- **golden image** an image on top of which developers can build applications, letting them focus on the application itself instead of system dependencies and patches.
	-  A typical golden image includes common system, logging, and monitoring tools, recent security patches, and application dependencies.
### why use packer?
- each cloud provider has their own platforms for running your workload
	- each of these platforms require its own type of image
- packer aims to create golden images across different platforms
- packer build to build a new image or a new artifact from a repository
	- artifacts can be scripts, files, etc.
- then packer build will build identical images across all of these platforms we use

#### maintaining consistency across workloads
- core machine image is a custom image that all of our applications get deployed upon
	- then we provision workloads from it 
- can use our core machine image to generate a new image that may be needed for custom applications or meet certain configuration and requirements
	- still some level of consistency 

#### building your ideal image
- needs a base image for us to start from
	- could be a manufacture image that we customize
- could be an image from a cloud provider too or .iso
- within our packer build
	- could add in specific files
		- e.g. scripts, files, tools
	- can tie packer together with configuration management tools
	- can install updates
	- pull in secrets or use secrets

### packer
- open source and enterprise versions

#### what is packer?
- open source machine image creation tool
- automates the installation and configuration on packer made images
- works with multiple platforms
	- even from same configuration
- eliminates manual steps for golden image creation 
- works with docker, aws, azure, vmware, open stack, google cloud, digital ocean

#### main use
- create our golden image across platforms and environment
- we want identical images on all our environments
	- environment meaning like DEV, QA, production, etc.
- workloads run on top of the packer images
- packer popular for creating an "image factory"
	- goes along with continuous delivery
	- as our application has new requirements, we can simply plug those into packer template in our repo 
		- then it can build a new image then our app can grab the latest packer image to deploy the workload on
- could automate monthly patching 
	- can rebuild our application using the packer image that we already downloaded the latest updates on already

### packer commands
Packer commands will also look familiar to you having worked with Terraform.
To see a list of packer commands and short description run

```
packer -h
```

You should see something like this:

```
Usage: packer [--version] [--help] <command> [<args>]

Available commands are:
    build           build image(s) from template
    console         creates a console for testing variable interpolation
    fix             fixes templates from old versions of packer
    fmt             Rewrites HCL2 config files to canonical format
    hcl2_upgrade    transform a JSON template into an HCL2 configuration
    init            Install missing plugins or upgrade plugins
    inspect         see components of a template
    plugins         Interact with Packer plugins and catalog
    validate        check that a template is valid
    version         Prints the Packer version
```

Download and install plugins in the 'required_plugins' block

```
packer init .
```

Unlike Terraform by default this installs plugins in `~/.config/packer/plugins` (on Unix like OSs).

You can validate the syntax of your Packer template with the command

```
packer validate <my-template.pkr.hcl>
```

Generate artifacts from the template

```
packer build
```

### writing packer templates
Packer templates are made up of blocks.

#### packer[](#packer)

The "packer" block contains the "required_plugins" block, includes plugins that allow you to build images for different providers. This includes cloud service providers like AWS as well as other platforms like docker.

This is similar to the required providers block in Terraform.

#### locals and variable blocks[](#locals-and-variable-blocks)

These are similar to locals and variables in Terraform.

**Reference:**

- [Input Variables and local variables](https://developer.hashicorp.com/packer/guides/hcl/variables)

#### source[](#source)

The "source" block specifies the original image that you are starting with. This could be an AWS AMI, a container image...

#### build[](#build)

The "build" block defines what packer should do with the image when it is launched. Packer creates a new image by automating the process of configuring an image. You do all of the things you regularly do to configure an image. Upload files, run shell scripts, use configuration tools like Ansible... After configuring the image packer saves the image somewhere. In the case of AWS, Packer will create a new AMI and add it to your AWS account.

The build block accomplishes the actual image building using "provisioners".

#### Packer provisioners[](#packer-provisioners)

Hashicorp maintains several provisioners, including "file" and "shell" provisioners. These come included with Packer. There are also several provisioners that are maintained as separate plugins.

Today we are going to look at the file and shell provisioners. Later we will look at the Ansible provisioner.

> [!note] Provisioner blocks go inside of the build block like in the example below

```
build {
  sources = [
    "source.amazon-ebs.ubuntu"
  ]

  provisioner "shell" {
    inline = [
      "echo Installing Updates",
      "sudo apt-get update",
      "sudo apt-get upgrade -y",
      "sudo apt-get install -y nginx"
    ]
  }
}
```

#### file[](#file)

The file provisioner allows you to add files to the image built by Packer.

```
provisioner "file" {
  source = "app.tar.gz"
  destination = "/tmp/app.tar.gz"
}
```

You can only upload files to locations that the default user has permission to write files to. For example, we have been using Debian in AWS AMI instances. The default user is the "admin" user, not root.

A common practice is to write files and directories to /tmp or the users home directory and move them, change permissions, ownership with shell provisioners.

#### shell[](#shell)

The shell provisioner allows you to run inline commands and shell scripts on the base image, to configure the new image you are creating.

You will commonly see packer templates with multiple shell provisioners.

Example of some in line commands and setting an environment variable

```
provisioner "shell" {
  environment_vars = [
    "FOO=hello world",
  ]

  inline = [
    "echo Installing Redis",
    "sleep 30",
    "sudo apt-get update",
    "sudo apt-get install -y redis-server",
    "echo \"FOO is $FOO\" > example.txt",
  ]
}
```

example of running a shell script

```
provisioner "shell" {
  script       = "script.sh"
  pause_before = "10s"
  timeout      = "10s"
}
```

There is also a shell-local provisioner for running commands on your local machine. This can be useful for writing data on artifacts to local files

**Reference:**

- The [file](https://developer.hashicorp.com/packer/docs/provisioners/file) provisioner
- The [shell](https://developer.hashicorp.com/packer/docs/provisioners/shell) provisioner
- [Packer Docs, HCL Templates](https://developer.hashicorp.com/packer/docs/templates/hcl_templates)

### Cleaning up[](#cleaning-up)

Creating a new AMI will create a new AMI and a new Snapshot in your AWS account.

To clean up these resources Deregister the AMI and select the Delete Snapshot option when deregistering.

### Packer questions
1. which can you use a packer image for?
	- clou,d VM
	- containers (docker, padman, etc)
	- on premise server VM
2. T or F: use single packer i,age template to build images for multiple platforms? T
3. what do these do?
	* source: original image (ubuntu, debian, etc.) that we will base it on
	* build: plug ins. defines the new images 
	* provider: install packages and plug ins (files, ansible, shell scripts, etc.)
4. how to enable and configure logging in packer and terraform?
	- environment variables 
	- need to set logging on and set it to a certain level
5. how does packer communicate with a source AWS image when creating a new image?
	* communicator like win RM or SSH
	* mainly SSH
6. what does it do with the source instance after your create the new image?
	* the instance is deleted 
	* it cleans up everything for us so we don't have extra VMs around