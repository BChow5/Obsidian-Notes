### Linux boot process 
- init
	- systemd, runit, r6. openRC
- **Reference:**
	- [The Arch boot process](https://wiki.archlinux.org/title/Arch_boot_process)

### systemd
> systemd is a system and service manager for Linux operating systems. When run as first process on boot (as PID 1), it acts as init system that brings up and maintains userspace services. Separate instances are started for logged-in users to start their services. systemd is usually not invoked directly by the user, but is installed as the /sbin/init symlink and started during early boot. The user manager instances are started automatically through the [user@.service](mailto:user@.service)(5) service. -from the man page

- what is a service manager? 
	- manages the services on boot up 
- what are services? 
	- they start up and wait for something 

systemd provides several tools that perform general tasks required by an operating system. This includes things like logging, a boot manager and an init system.

An 'init' (short for initialization) tool is a tool that is used to manage daemons.

An init system is generally the first process started by your system, PID 1. You can confirm this by running the command:

```
ps -p 1
```

## services

A service is what systemd refers to daemons as.

A service is any "background" process, running without a user interface. Services generally wait for events to occur that trigger some response. Two examples of this are:

- a web server, which waits for a request from a client before returning a response
- sshd which waits for a remote user to login via ssh.

## [](#targets)targets

A target is a group of services that are attached through dependencies that start all of the services required to meet some sort of system state. eg:
- targets are like a collection of services 

- multi-user.target, which is when all of the services needed for a minimal working server have started
- graphical.target, which is when all of the services needed for graphical login have started. When you can login through a graphical login manager.

Some targets function by building on top of other targets. For example the graphical.target first loads the multi-user.target and adds several additional dependencies to this.


You can see current targets with the command:

```
systemctl list-units --type=target
```

**Reference:**

[https://systemd.io/](https://systemd.io/)

[https://wiki.archlinux.org/title/Systemd](https://wiki.archlinux.org/title/Systemd)

## [](#systemctl-managing-services)`systemctl`, managing services

> systemctl may be used to introspect and control the state of the "systemd" system and service manager. Please refer to systemd(1) for an introduction into the basic concepts and functionality this tool manages.
> 
> - From the man page

in other words, systemctl is a tool that is used to manage services in many Linux operating systems.

The `systemctl` command can be used to find information about services, to start and stop services, and to enable (start a service on boot) and disable a service.

## [](#systemctl-command-examples)`systemctl` command examples

| Action                                                                        | Command                                    | Note                                                                                  |
| ----------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| Analyzing the system state                                                    |                                            |                                                                                       |
| **Show system status** *                                                      | `systemctl status`                         |                                                                                       |
| **List running** units                                                        | `systemctl` or  <br>`systemctl list-units` |                                                                                       |
| **List failed** units                                                         | `systemctl --failed`                       |                                                                                       |
| **List installed** unit files1                                                | `systemctl list-unit-files`                |                                                                                       |
| **Show process status** for a PID                                             | `systemctl status _pid_`                   | [cgroup slice](https://wiki.archlinux.org/title/Cgroups "Cgroups"), memory and parent |
| Checking the unit status                                                      |                                            |                                                                                       |
| **Show a manual page** associated with a unit                                 | `systemctl help _unit_`                    | as supported by the unit                                                              |
| **Status** of a unit                                                          | `systemctl status _unit_`                  | including whether it is running or not                                                |
| **Check** whether a unit is enabled                                           | `systemctl is-enabled _unit_`              |                                                                                       |
| Starting, restarting, reloading a unit                                        |                                            |                                                                                       |
| **Start** a unit immediately                                                  | `systemctl start _unit_` as root           |                                                                                       |
| **Stop** a unit immediately                                                   | `systemctl stop _unit_` as root            |                                                                                       |
| **Restart** a unit                                                            | `systemctl restart _unit_` as root         |                                                                                       |
| **Reload** a unit and its configuration                                       | `systemctl reload _unit_` as root          |                                                                                       |
| **Reload systemd manager** configuration2                                     | `systemctl daemon-reload` as root          | scan for new or changed units                                                         |
| Enabling a unit                                                               |                                            |                                                                                       |
| **Enable** a unit to start automatically at boot                              | `systemctl enable _unit_` as root          |                                                                                       |
| **Enable** a unit to start automatically at boot and **start** it immediately | `systemctl enable --now _unit_` as root    |                                                                                       |
| **Disable** a unit to no longer start at boot                                 | `systemctl disable _unit_` as root         |                                                                                       |
| **Reenable** a unit3                                                          | `systemctl reenable _unit_` as root        | i.e. disable and enable anew                                                          |
| Masking a unit                                                                |                                            |                                                                                       |
| **Mask** a unit to make it impossible to start                                | `systemctl mask _unit_` as root            |                                                                                       |
| **Unmask** a unit                                                             | `systemctl unmask _unit_` as root          |                                                                                       |

## [](#unit-files)unit files

A unit file is a plain text ini-style file that encodes information about a service, a socket, a device, a mount point, an automount point, a swap file or partition, a start-up target, a watched file system path, a timer controlled and supervised by [systemd(1)](https://man.archlinux.org/man/systemd.1.en), a resource management slice or a group of externally created processes. See [systemd.syntax(7)](https://man.archlinux.org/man/systemd.syntax.7.en) for a general description of the syntax.

This man page lists the common configuration options of all the unit types. These options need to be configured in the [Unit] or [Install] sections of the unit files.

In addition to the generic [Unit] and [Install] sections described here, each unit may have a type-specific section, e.g. [Service] for a service unit. See the respective man pages for more information: [systemd.service(5)](https://man.archlinux.org/man/systemd.service.5.en), [systemd.socket(5)](https://man.archlinux.org/man/systemd.socket.5.en), [systemd.device(5)](https://man.archlinux.org/man/systemd.device.5.en), [systemd.mount(5)](https://man.archlinux.org/man/systemd.mount.5.en), [systemd.automount(5)](https://man.archlinux.org/man/systemd.automount.5.en), [systemd.swap(5)](https://man.archlinux.org/man/systemd.swap.5.en), [systemd.target(5)](https://man.archlinux.org/man/systemd.target.5.en), [systemd.path(5)](https://man.archlinux.org/man/systemd.path.5.en), [systemd.timer(5)](https://man.archlinux.org/man/systemd.timer.5.en), [systemd.slice(5)](https://man.archlinux.org/man/systemd.slice.5.en), [systemd.scope(5)](https://man.archlinux.org/man/systemd.scope.5.en).

### [](#locations-of-unit-files)Locations of unit files

|Directory|Description|
|---|---|
|`/usr/lib/systemd/system/`|Systemd unit files distributed with installed RPM packages.|
|`/run/systemd/system/`|Systemd unit files created at run time. This directory takes precedence over the directory with installed service unit files.|
|`/etc/systemd/system/`|Systemd unit files created by `systemctl enable` as well as unit files added for extending a service. This directory takes precedence over the directory with runtime unit files.|

## [](#types-of-units)Types of units

We are only going to look at _services_ and _timers_ this term.

- **`.service`**: A service unit describes how to manage a service or application on the server. This will include how to start or stop the service, under which circumstances it should be automatically started, and the dependency and ordering information for related software.
- **`.socket`**: A socket unit file describes a network or IPC socket, or a FIFO buffer that `systemd` uses for socket-based activation. These always have an associated `.service` file that will be started when activity is seen on the socket that this unit defines.
- **`.device`**: A unit that describes a device that has been designated as needing `systemd` management by `udev` or the `sysfs` filesystem. Not all devices will have `.device` files. Some scenarios where `.device` units may be necessary are for ordering, mounting, and accessing the devices.
- **`.mount`**: This unit defines a mountpoint on the system to be managed by `systemd`. These are named after the mount path, with slashes changed to dashes. Entries within `/etc/fstab` can have units created automatically.
- **`.automount`**: An `.automount` unit configures a mountpoint that will be automatically mounted. These must be named after the mount point they refer to and must have a matching `.mount` unit to define the specifics of the mount.
- **`.swap`**: This unit describes swap space on the system. The name of these units must reflect the device or file path of the space.
- **`.target`**: A target unit is used to provide synchronization points for other units when booting up or changing states. They also can be used to bring the system to a new state. Other units specify their relation to targets to become tied to the target’s operations.
- **`.path`**: This unit defines a path that can be used for path-based activation. By default, a `.service` unit of the same base name will be started when the path reaches the specified state. This uses `inotify` to monitor the path for changes.
- **`.timer`**: A `.timer` unit defines a timer that will be managed by `systemd`, similar to a `cron` job for delayed or scheduled activation. A matching unit will be started when the timer is reached.
- **`.snapshot`**: A `.snapshot` unit is created automatically by the `systemctl snapshot` command. It allows you to reconstruct the current state of the system after making changes. Snapshots do not survive across sessions and are used to roll back temporary states.
- **`.slice`**: A `.slice` unit is associated with Linux Control Group nodes, allowing resources to be restricted or assigned to any processes associated with the slice. The name reflects its hierarchical position within the `cgroup` tree. Units are placed in certain slices by default depending on their type.
- **`.scope`**: Scope units are created automatically by `systemd` from information received from its bus interfaces. These are used to manage sets of system processes that are created externally.

## [](#unit-file-syntax)Unit file syntax

Unit files or ogranized in sections. Each section is wrapped in square brackets. Section names are case sensitive

Inside of each section data is provided with directives Directives are key value pairs

```
[Section]
directive=value
```

Generally a unit file will include three sections, _Unit_ and _Install_ as well as one section of the type of unit that file creates, ie a Service section in a .service unit or Timer section in a .timer unit

Unit files end in the file suffix that indicates what type of unit it is. So a service, will be unit-name.service

**Example of a service file**

```
[Unit]
Description=Some HTTP server
After=remote-fs.target sqldb.service
Requires=sqldb.service
AssertPathExists=/srv/webserver

[Service]
Type=notify
ExecStart=/usr/sbin/some-fancy-httpd-server

[Install]
WantedBy=multi-user.target
```

### [](#handling-dependencies)Handling dependencies

> With _systemd_, dependencies can be resolved by designing the unit files correctly. The most typical case is when unit _A_ requires unit _B_ to be running before _A_ is started. In that case add `Requires=_B_` and `After=_B_` to the `[Unit]` section of _A_. If the dependency is optional, add `Wants=_B_` and `After=_B_` instead. Note that `Wants=` and `Requires=` do not imply `After=`, meaning that if `After=` is not specified, the two units will be started in parallel.
> 
> - [Arch Wiki](https://wiki.archlinux.org/title/Systemd)

**`WantedBy=`**: The `WantedBy=` directive is the most common way to specify how a unit should be enabled. This directive allows you to specify a dependency relationship in a similar way to the `Wants=` directive does in the `[Unit]` section. The difference is that this directive is included in the ancillary unit allowing the primary unit listed to remain relatively clean. When a unit with this directive is enabled, a directory will be created within `/etc/systemd/system` named after the specified unit with `.wants` appended to the end. Within this, a symbolic link to the current unit will be created, creating the dependency. For instance, if the current unit has `WantedBy=multi-user.target`, a directory called `multi-user.target.wants` will be created within `/etc/systemd/system` (if not already available) and a symbolic link to the current unit will be placed within. Disabling this unit removes the link and removes the dependency relationship.