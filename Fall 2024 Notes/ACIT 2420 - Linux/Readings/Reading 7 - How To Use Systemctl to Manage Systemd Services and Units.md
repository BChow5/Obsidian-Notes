- `systemd` is an init system and system manager that has widely become the new standard for Linux distributions
- `systemctl` command is the central management tool for controlling the init system

### Service Management
- fundamental purpose on an init system is to initialize the components that must be started after the Linux kernel is booted
- init system also used to manage services and daemons for the server at any point while system is running 
- In `systemd`, the target of most actions are “units”, which are resources that `systemd` knows how to manage
	- units are categorized by the type of resource they represent and they are defined with files known as unit files
	- the type of each unit can be inferred from the suffix on the end of the file
- service management tasks taret unit is service units
	- unit files with a suffix of `.service`
	- for most service management commands, you can leave off the suffix

#### Starting and Stopping Services
- to start a `systemd` service, executing instructions in the service's unit file, use the `start` command 
- if  you are running as a non-root user you will need sudo

```bash
sudo systemctl start application.service
```

```bash
sudo systemctl start application
```

- to stop a currently running service, use `stop`

```bash
sudo systemctl stop application.service
```

#### Restarting and Reloading
- use `restart` command to restart a running service

```bash
sudo systemctl restart application.service
```

- some programs can also reload its configuration files with `reload` command

```bash
sudo systemctl reload application.service
```

- If you are unsure whether the service has the functionality to reload its configuration, you can issue the `reload-or-restart` command

```bash
sudo systemctl reload-or-restart application.service
```

#### Enabling and Disabling Services
- to start a service at boot, use `enable` command

```bash
sudo systemctl enable application.service
```
- This will create a symbolic link from the system’s copy of the service file (usually in `/lib/systemd/system` or `/etc/systemd/system`) into the location on disk where `systemd` looks for autostart files (usually `/etc/systemd/system/==some_target==.target.wants`

- use `disable` to disable service from starting automatically

```bash
sudo systemctl disable application.service
```
- This will remove the symbolic link that indicated that the service should be started automatically.
- Keep in mind that enabling a service does not start it in the current session. If you wish to start the service and also enable it at boot, you will have to issue both the `start` and `enable` commands

#### Checking the Status of Services
- check status of services with `status` command

```bash
systemctl status application.service
```
- This will provide you with the service state, the cgroup hierarchy, and the first few log lines.

- There are also methods for checking for specific states. For instance, to check to see if a unit is currently active (running), you can use the `is-active` command:
```bash
systemctl is-active application.service
```
- This will return the current unit state, which is usually `active` or `inactive`. The exit code will be “0” if it is active, making the result simpler to parse in shell scripts.

- To see if the unit is enabled, you can use the `is-enabled` command:
```bash
systemctl is-enabled application.service
```
- This will output whether the service is `enabled` or `disabled` and will again set the exit code to “0” or “1” depending on the answer to the command question.

- check is whether the unit is in a failed state. This indicates that there was a problem starting the unit in question:
```bash
systemctl is-failed application.service
```
- This will return `active` if it is running properly or `failed` if an error occurred. If the unit was intentionally stopped, it may return `unknown` or `inactive`. 
- An exit status of “0” indicates that a failure occurred and an exit status of “1” indicates any other status

### System State Overview

#### Listing Current Units
- To see a list of all of the active units that `systemd` knows about, we can use the `list-units` command:
```bash
systemctl list-units
```

The output has the following columns:

- **UNIT**: The `systemd` unit name
- **LOAD**: Whether the unit’s configuration has been parsed by `systemd`. The configuration of loaded units is kept in memory.
- **ACTIVE**: A summary state about whether the unit is active. This is usually a fairly basic way to tell if the unit has started successfully or not.
- **SUB**: This is a lower-level state that indicates more detailed information about the unit. This often varies by unit type, state, and the actual method in which the unit runs.
- **DESCRIPTION**: A short textual description of what the unit is/does.
- Since the `list-units` command shows only active units by default, all of the entries above will show `loaded` in the LOAD column and `active` in the ACTIVE column.

- This display is actually the default behavior of `systemctl` when called without additional commands, so you will see the same thing if you call `systemctl` with no arguments
- We can tell `systemctl` to output different information by adding additional flags. 
- For instance, to see all of the units that `systemd` has loaded (or attempted to load), regardless of whether they are currently active, you can use the `--all` flag, like this:
```bash
systemctl list-units --all
```

- You can use other flags to filter these results. For example, we can use the `--state=` flag to indicate the LOAD, ACTIVE, or SUB states that we wish to see. You will have to keep the `--all` flag so that `systemctl` allows non-active units to be displayed
```bash
systemctl list-units --all --state=inactive
```

```bash
systemctl list-units --type=service
```

#### Listing All Unit Files
- To see _every_ available unit file within the `systemd` paths, including those that `systemd` has not attempted to load, you can use the `list-unit-files` command instead:

```bash
systemctl list-unit-files
```

- Units are representations of resources that `systemd` knows about. Since `systemd` has not necessarily read all of the unit definitions in this view, it only presents information about the files themselves. The output has two columns: the unit file and the state.
![[Pasted image 20241103125247.png]]
- The state will usually be `enabled`, `disabled`, `static`, or `masked`. In this context, static means that the unit file does not contain an `install` section, which is used to enable a unit

### Unit Management

#### Displaying a Unit File
- To display the unit file that `systemd` has loaded into its system, you can use the `cat` command

```bash
systemctl cat atd.service
```
![[Pasted image 20241103125829.png]]
- The output is the unit file as known to the currently running `systemd` process

#### Displaying Dependencies 
- The output is the unit file as known to the currently running `systemd` process
```bash
systemctl list-dependencies sshd.service
```
- This will display a hierarchy mapping the dependencies that must be dealt with in order to start the unit in question. 
	- Dependencies, in this context, include those units that are either required by or wanted by the units above it.
![[Pasted image 20241103130146.png]]
- The recursive dependencies are only displayed for `.target` units, which indicate system states. To recursively list all dependencies, include the `--all` flag.
- To show reverse dependencies (units that depend on the specified unit), you can add the `--reverse` flag to the command. Other flags that are useful are the `--before` and `--after` flags, which can be used to show units that depend on the specified unit starting before and after themselves, respectively

#### Checking Unit Properties
- To see the low-level properties of a unit, you can use the `show` command. This will display a list of properties that are set for the specified unit using a `key=value` format:

```bash
systemctl show sshd.service
```
![[Pasted image 20241103130617.png]]
- If you want to display a single property, you can pass the `-p` flag with the property name.
```bash
systemctl show sshd.service -p Conflicts
```
![[Pasted image 20241103130650.png]]

#### Masking and Unmasking Units

- `systemd` also has the ability to mark a unit as _completely_ unstartable, automatically or manually, by linking it to `/dev/null`. This is called masking the unit, and is possible with the `mask` command:
```bash
sudo systemctl mask nginx.service
```
- This will prevent the Nginx service from being started, automatically or manually, for as long as it is masked.
- If you check the `list-unit-files`, you will see the service is now listed as masked:

- To unmask a unit, making it available for use again, use the `unmask` command:
```bash
sudo systemctl unmask nginx.service
```

### Editing Unit Files
- The `edit` command, by default, will open a unit file snippet for the unit in question:
```bash
sudo systemctl edit nginx.service
```
- This will be a blank file that can be used to override or add directives to the unit definition
- A directory will be created within the `/etc/systemd/system` directory which contains the name of the unit with `.d` appended. For instance, for the `nginx.service`, a directory called `nginx.service.d` will be created.
- Within this directory, a snippet will be created called `override.conf`. 
- When the unit is loaded, `systemd` will, in memory, merge the override snippet with the full unit file. 
- The snippet’s directives will take precedence over those found in the original unit file.

- If you wish to edit the full unit file instead of creating a snippet, you can pass the `--full` flag:
```bash
sudo systemctl edit --full nginx.service
```
- This will load the current unit file into the editor, where it can be modified. When the editor exits, the changed file will be written to `/etc/systemd/system`, which will take precedence over the system’s unit definition (usually found somewhere in `/lib/systemd/system`).

To remove any additions you have made, either delete the unit’s `.d` configuration directory or the modified service file from `/etc/systemd/system`. For instance, to remove a snippet, we could type:

```
sudo rm -r /etc/systemd/system/nginx.service.d
```

To remove a full modified unit file, we would type:

```
sudo rm /etc/systemd/system/nginx.service
```

After deleting the file or directory, you should reload the `systemd` process so that it no longer attempts to reference these files and reverts back to using the system copies. You can do this by typing:

```
sudo systemctl daemon-reload
```

### Adjusting the System State (Runlevel) with Targets

- Targets are special unit files that describe a system state or synchronization point. 
	- Like other units, the files that define targets can be identified by their suffix, which in this case is `.target`. 
	- Targets do not do much themselves, but are instead used to group other units together.
- This can be used in order to bring the system to certain states, much like other init systems use runlevels. 
	- They are used as a reference for when certain functions are available, allowing you to specify the desired state instead of the individual units needed to produce that state.

For instance, there is a `swap.target` that is used to indicate that swap is ready for use. Units that are part of this process can sync with this target by indicating in their configuration that they are `WantedBy=` or `RequiredBy=` the `swap.target`. Units that require swap to be available can specify this condition using the `Wants=`, `Requires=`, and `After=` specifications to indicate the nature of their relationship.

#### Getting and Setting the Default Target
- The `systemd` process has a default target that it uses when booting the system
- To find the default target for your system, type:
```bash
systemctl get-default
```
- If you wish to set a different default target, you can use the `set-default`.
```bash
sudo systemctl set-default graphical.target
```

#### Listing Available Targets
- You can get a list of the available targets on your system by typing:
```bash
systemctl list-unit-files --type=target
```
- Unlike runlevels, multiple targets can be active at one time. An active target indicates that `systemd` has attempted to start all of the units tied to the target and has not tried to tear them down again.

- To see all of the active targets, type:
```bash
systemctl list-units --type=target
```

#### Isolating Targets
- possible to start all of the units associated with a target and stop all units that are not part of the dependency tree
	- use `isolate` for this

For instance, if you are operating in a graphical environment with `graphical.target` active, you can shut down the graphical system and put the system into a multi-user command line state by isolating the `multi-user.target`. Since `graphical.target` depends on `multi-user.target` but not the other way around, all of the graphical units will be stopped.

You may wish to take a look at the dependencies of the target you are isolating before performing this procedure to ensure that you are not stopping vital services:

```
systemctl list-dependencies multi-user.target
```


When you are satisfied with the units that will be kept alive, you can isolate the target by typing:

```
sudo systemctl isolate multi-user.target
```

### [Using Shortcuts for Important Events](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units#using-shortcuts-for-important-events)[](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units#using-shortcuts-for-important-events)

There are targets defined for important events like powering off or rebooting. However, `systemctl` also has some shortcuts that add a bit of additional functionality.

For instance, to put the system into rescue (single-user) mode, you can use the `rescue` command instead of `isolate rescue.target`:

```
sudo systemctl rescue
```


This will provide the additional functionality of alerting all logged in users about the event.

To halt the system, you can use the `halt` command:

```
sudo systemctl halt
```


To initiate a full shutdown, you can use the `poweroff` command:

```
sudo systemctl poweroff
```


A restart can be started with the `reboot` command:

```
sudo systemctl reboot
```


These all alert logged in users that the event is occurring, something that only running or isolating the target will not do. Note that most machines will link the shorter, more conventional commands for these operations so that they work properly with `systemd`.

For example, to reboot the system, you can usually type:

```
sudo reboot
```


## [Conclusion](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units#conclusion)[](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units#conclusion)

By now, you should be familiar with some of the basic capabilities of the `systemctl` command that allow you to interact with and control your `systemd` instance. The `systemctl` utility will be your main point of interaction for service and system state management.

While `systemctl` operates mainly with the core `systemd` process, there are other components to the `systemd` ecosystem that are controlled by other utilities. Other capabilities, like log management and user sessions are handled by separate daemons and management utilities (`journald`/`journalctl` and `logind`/`loginctl` respectively). Taking time to become familiar with these other tools and daemons will make management an easier task.

Want to easily configure a performant, secure, and stable server? Try DigitalOcean Droplets! Simple enough for any user, powerful enough for fast-growing applications or businesses.
