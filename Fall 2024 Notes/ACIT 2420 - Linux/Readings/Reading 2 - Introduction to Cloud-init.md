- cloud-init is an open source initialization tool that was designed to make it better to make it easier to get your systems up and running with a minimum effort, already configured according to your needs

### What is the benefit of cloud-init?
- when you deploy a new cloud instance, cloud-init takes an initial configuration that you supply, and it automatically applies those settings when the instance is created
	- its like writing a to-do list and letting cloud-init deal with the list for you
- the real power of cloud-init comes from the fact that you can re-use your configuration instructions as often as you want and always get consistent, reliable results
- if you're a system admin, this is great to deploy a whole fleet of machines quickly 

### What does cloud-init do?
- cloud-init can handle a range of tasks that normally happen when a new instance is created
- it's responsible for activities like setting hostname, configuring network itnerfaces, creating user accounts, and even running scipts
- this streamlines the deployment process
- your cloud instances will be automatically configured the same way, which reduces the change for human error 

### How does cloud-init work?
- the operation of cloud-init broadly takes place in two separate phases during the boot process
	- first phase is during the early (local) boot stage, before networking has been enabled
	- the second is during the late boot stages, after cloud-init has applied the networking configuration 

#### During early boot
- in this pre-networking stage, cloud-init discoerts the data source, obtains all the configuration data from it, and configures networking
- in this phase it will:
- **identify the data source**
	- the hardware is checked for built-in values that will identify the datasource your instance is running on
	- the data source is the source of all configuration data
- **fetch the configuration**
	- once the data source is identified, cloud-init fetches the configuration data from it
	- this data tells cloud-init what actions to take
	- this can be in the form of:
		- **meta data** about the instance, such as machine ID, hostname, and network config
		- **vendor data** and/or **user data**. these take the same form
			- althoguh vendor data is provided by the cloud vendor and user data is provided by the user
			- these data are usually applied in the post-network phase and might include:
				- hardware optimizations
				- integrations with the specific cloud platform
				- SSH keys
				- custom scripts
- **write network configuration**
	- cloud-init writes the network configuration and configures DNS, ready to be applied by the networking services when they come up 

#### During late boot 
- in the boot stage after the network has been confiugred, cloud-init runs through the tasks that were not critical for provisioning
- this is where it configures the running instance according to your needs, as specified in the vendor and/or user data
- it will take care of:
	- configuration management
		- cloud-init can interact with tools like puppet, ansible, or chef to apply more complex configuration and ensure the system is up to date
	- installing software
		- can install software at this stage, and run software updates to make sure the system is fully up to date
	- user accounts
		- able to crate and modify user accounts, set default passwords, and configure permissions
	- execute user scripts
		- if any custom scripts were provided in the user data, cloud-init can run them
		- this allows additional specified software to be installed
		- it can also inject SSG keys into the interface's authorized keys file which allows secure remote access to the machine
		- 