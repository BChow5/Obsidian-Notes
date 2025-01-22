# package managers

There are Windows and MacOS package managers as well. They are worth looking into for developers who want to use Windows or MacOS.

## [](#linux-package-managers)Linux package managers

A package manager is a tool that makes it easy for a user to manage software on their system.

Package managers generally allow you perform operations like installing, removing and updating software.

One advantage to using a package manager is that they generally handle dependencies. If a package requires additional software to run, the package manager will install that as well. They also usually install things like default configuration files and man pages.

Package managers are one of the few tools that make one Linux distro different from another Linux distro.

Package managers interact with package repositories, remote repositories where binaries, or build instructions for software exists.

Some Linux distros include several utilities that can be used to manage packages. The list below just includes the most commonly used utility for those distros.

|distro|package manager|
|---|---|
|Alpine|apk|
|Arch|pacman|
|Debian|apt|
|Fedora|dnf|
|Nix|nix|
|openSuse|zypper|
|Rocky|dnf|
|Ubuntu|apt ( or snaps )|
|There have been several attempts at creating a "Universal package manager" like [Flatpak](https://flatpak.org/). Many Linux users will install Flatpak and install packages using a combination of the default package manager and Flatpak.||

In addition to the package manager itself, different distros will have different packages in their package repositories. This means that you might be able to find a package in one package repository but not in another. So while you might be able to install the [river](https://codeberg.org/river/river) [wayland](https://wayland.freedesktop.org/) compositor on [Alpine Linux](https://www.alpinelinux.org/) with the package manager you won't be able to install it on [Chimera Linux](https://chimera-linux.org/). Even though both distros use the same package manager. They maintain their own package repositories. The other thing to watch out for is different package names in different repositories. The `fd` utility for example. On some distros the package name is 'fd', on others it is 'fd-find'. Because of this it is a good idea to search for a package before installing it.

You can install packages by building the software locally as well.

One of the reasons that Arch Linux is so popular is that it has a lot of packages, and those packages are generally up-to-date.

## [](#pacman)pacman

> The [pacman](https://pacman.archlinux.page/) [package manager](https://en.wikipedia.org/wiki/Package_manager "wikipedia:Package manager") is one of the major distinguishing features of [Arch Linux](https://wiki.archlinux.org/title/Arch_Linux "Arch Linux"). It combines a simple binary package format with an easy-to-use [Arch build system](https://wiki.archlinux.org/title/Arch_build_system "Arch build system"). The goal of _pacman_ is to make it possible to easily manage packages, whether they are from the [official repositories](https://wiki.archlinux.org/title/Official_repositories "Official repositories") or the user's own builds.
> 
> - arch wiki

## [](#pacman-commands)pacman commands

This list below is a good place to start for day to day use, but doesn't include every `pacman` command.

| command                    | Action                                                         |
| -------------------------- | -------------------------------------------------------------- |
| `pacman -Sy`               | Refresh the local package repository                           |
| `pacman -Syu`              | Upgrade installed packages                                     |
| `pacman -S <packages>`     | Install one or more packages and any dependencies              |
| `pacman -Ss <search term>` | Search for a package                                           |
| `pacman -Rns <packages>`   | Remove a package, configuration files and package dependencies |
| `pacman -Si <package>`     | Display remote information about a package                     |

### [](#pacman-configuration-and-repositories)pacman configuration and repositories

configuration for pacman is in `/etc/pacman.conf` the mirrors list is in `/etc/pacman/pacman.d/mirrorlist`

Above I mentioned package repositories.

> A [software repository](https://en.wikipedia.org/wiki/software_repository "wikipedia:software repository") is a storage location from which software packages are retrieved for installation. Arch Linux **official repositories** contain essential and popular software, readily accessible via [pacman](https://wiki.archlinux.org/title/Pacman "Pacman"). They are maintained by [package maintainers](https://wiki.archlinux.org/title/Arch_terminology#Package_maintainer "Arch terminology"). Packages in the official repositories are constantly upgraded: when a package is upgraded, its old version is removed from the repository. There are no major Arch releases: each package is upgraded as new versions become available from upstream sources. Each repository is always coherent, i.e. the packages that it hosts always have reciprocally compatible versions.
> 
> - [Arch Wiki](https://wiki.archlinux.org/title/Official_repositories)

These are are often a git repository that either contains binaries (compiled applications that can be installed), or build instructions for software that build and install a package from source.

pacman has several package repositories enabled by default as well as additional repositories that can be enabled to install the unstable, bleeding edge, versions of some software. Or specific less common software.

a mirror of a repository is a copy of a remote repository and you might want to use them since they could be closer 

You can see which package repositories are enabled by looking at `/etc/pacman.conf`

> [!Question] Which repositories are enabled on your server?

Arch Linux also has the AUR, Arch User Repository. This is a collection of user submitted packages for Arch Linux. If a package you are looking for is not in the official package repositories you can look for it in the AUR.

By default you don't install packages from the AUR the same way you do official packages.

### [](#looking-at-a-package)looking at a package.

We can easily inspect a package by going to the Arch Linux site and searching for package in the web interface. For example kakoune [https://archlinux.org/packages/?sort=&q=kakoune&maintainer=&flagged=](https://archlinux.org/packages/?sort=&q=kakoune&maintainer=&flagged=)

> [!Question] What information can you find looking at the package information in a browser?

**Reference:**

[Arch wiki pacman](https://wiki.archlinux.org/title/Pacman)

# [](#systemd-timers)systemd timers

## [](#timers)timers

A timer is a type of unit file that allows you to start a service at a specified time. This can be a relatively specific time, eg. the first Monday of the month at 04:00.

`man systemd.timer`

view running timers `systemctl list-timers`

**file naming convention**

It is recommended that you name timers using the same name as the service they start, you don't have to, but unless you have a good reason to not use the same name you should. You also have to add an additional line to a timer file if the name is different. If you use a different name in the Timer section, add `Unit=service-name.service` If you use the same name, you can leave this line out.

**Timers OnCalendar and OnUnitActiveSec**

systemd has two types of timers:

- **Monotonic** timers
- and **Realtime** timers

**Monotonic timer**, These start in relation to other system events, such as on Boot

```
[Unit]
Description=Run foo weekly and on boot

[Timer]
OnBootSec=15min
OnUnitActiveSec=20d

[Install]
WantedBy=timers.target
```

**Realtime timer**, these start on calendar events, 05:00 first day of the month for example

```
[Unit]
Description=Run foo weekly

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

OnCalendar event format An OnCalendar event uses a format similar to cron, it can take a little while to wrap your head around, but it isn’t actually too bad.

`DayOfWeek Year-Month-Day Hour:Minute:Second`

To run a service on the first Saturday of every month, at 18:00 use:

```
OnCalendar=Sat *-*-1..7 18:00:00
```

When using the `DayOfWeek` part, at least one weekday has to be specified. If you want something to run every day at 04:00 use:

```
OnCalendar=*-*-* 04:00:00
```

**testing your onCalendar (realtime) timers** before adding a time in a timer file it is useful to test out that the timer will run as you intend. systemd-analyze can be used for this as well.

```
systemd-analyze calendar "*-*-* 01:00:00"
```

**Reference:** [systemd/Timers - ArchWiki](https://wiki.archlinux.org/title/Systemd/Timers)

see `man systemd.timer` and `man systemd.time`

Just like services after creating a new timer you need to run the following command this checks for new unit files, or changes to unit files, it doesn't restart a service even if a change exists

`sudo systemctl daemon-reload`

## [](#timers-services-and-scripts-oh-my)timers, services and scripts oh my!

Something that tripped almost everyone up last week was not testing your script before testing everything altogether. **Test as you go**. Test that your script works. Test that you can start your script with your service, finally test that you can start your service with your timer.

![[Pasted image 20241112150817.png]]

### [](#changing-the-timezone)Changing the TimeZone

By default most servers are set to UTC so if you want your timer to start at 05:00 local time you either need to figure out the time difference, or set the time zone.

You can set the time zone in Arch(and a lot of other Linux distros) with this command

`sudo timedatectl set-timezone America/Vancouver`

You may have to restart your server after this.

check the current TZ with `timedatectl`

**Reference:**

- [Arch Wiki System time](https://wiki.archlinux.org/title/System_time#Time_zone)
- [current UTC time from time is](https://time.is/UTC)