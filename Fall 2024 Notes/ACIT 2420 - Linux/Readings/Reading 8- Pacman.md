- The [pacman](https://pacman.archlinux.page/) [package manager](https://en.wikipedia.org/wiki/Package_manager "wikipedia:Package manager") is one of the major distinguishing features of [Arch Linux](https://wiki.archlinux.org/title/Arch_Linux "Arch Linux").
- The goal of _pacman_ is to make it possible to easily manage packages, whether they are from the [official repositories](https://wiki.archlinux.org/title/Official_repositories "Official repositories") or the user's own builds.
- _Pacman_ keeps the system up-to-date by synchronizing package lists with the master server. This server/client model also allows the user to download/install packages with a simple command, complete with all required dependencies.

### Installing packages
- A package is an archive containing:
	- all of the (compiled) files of an application
	- metadata about the application, such as application name, version, dependencies, etc.
	- installation files and directives for _pacman_
- Arch's package manager _pacman_ can install, update, and remove those packages. Using packages instead of compiling and installing programs yourself has various benefits:
	- easily updatable: _pacman_ will update existing packages as soon as updates are available
	- dependency checks: _pacman_ handles dependencies for you, you only need to specify the program and _pacman_ installs it together with every other program it needs
	- clean removal: _pacman_ has a list of every file in a package; this way, no files are unintentionally left behind when you decide to remove a package.

**Note:**
- Packages often have [optional dependencies](https://wiki.archlinux.org/title/PKGBUILD#optdepends "PKGBUILD") which are packages that provide additional functionality to the application but not strictly required for running it. When installing a package, _pacman_ will list a package's optional dependencies, but they will not be found in `pacman.log`. Use the [#Querying package databases](https://wiki.archlinux.org/title/Pacman#Querying_package_databases) command to view the optional dependencies of a package.
- When installing a package which you require only as a (optional) dependency of some other package (i.e. not required by you explicitly), it is recommended to use the `--asdeps` option. For details, see the [#Installation reason](https://wiki.archlinux.org/title/Pacman#Installation_reason) section.

**Warning:** When installing packages in Arch, avoid refreshing the package list without [upgrading the system](https://wiki.archlinux.org/title/Pacman#Upgrading_packages) (for example, when a [package is no longer found](https://wiki.archlinux.org/title/Pacman#Packages_cannot_be_retrieved_on_installation) in the official repositories). In practice, do **not** run `pacman -Sy _package_name_` instead of `pacman -Sy**u** _package_name_`, as this could lead to dependency issues. See [System maintenance#Partial upgrades are unsupported](https://wiki.archlinux.org/title/System_maintenance#Partial_upgrades_are_unsupported "System maintenance") and [BBS#89328](https://bbs.archlinux.org/viewtopic.php?id=89328).

#### Installing specific packages

To install a single package or list of packages, including dependencies, issue the following command:

`# pacman -S _package_name1_ _package_name2_ ...`

To install a list of packages with regex (see [this forum thread](https://bbs.archlinux.org/viewtopic.php?id=7179)):

`# pacman -S $(pacman -Ssq _package_regex_)`

Sometimes there are multiple versions of a package in different repositories (e.g. _extra_ and _testing_). To install the version from the _extra_ repository in this example, the repository needs to be defined in front of the package name:

`# pacman -S extra/_package_name_`

To install a number of packages sharing similar patterns in their names, one can use curly brace expansion. For example:

`# pacman -S plasma-{desktop,mediacenter,nm}`

This can be expanded to however many levels needed:

`# pacman -S plasma-{workspace{,-wallpapers},pa}`

##### Virtual packages

- A virtual package is a special package which does not exist by itself, but is [provided](https://wiki.archlinux.org/title/PKGBUILD#provides "PKGBUILD") by one or more other packages. 
- Virtual packages allow other packages to not name a specific package as a dependency, in case there are several candidates. 
- Virtual packages cannot be installed by their name, instead they become installed in your system when you have installed a package _providing_ the virtual package. An example is the [dbus-units](https://wiki.archlinux.org/title/D-Bus#Implementations "D-Bus") package.
- 
#### Installing package groups

Some packages belong to a [group of packages](https://wiki.archlinux.org/title/Package_group "Package group") that can all be installed simultaneously. For example, issuing the command:

`# pacman -S gnome`

will prompt you to select the packages from the [gnome](https://archlinux.org/groups/x86_64/gnome/) group that you wish to install.

Sometimes a package group will contain a large amount of packages, and there may be only a few that you do or do not want to install. Instead of having to enter all the numbers except the ones you do not want, it is sometimes more convenient to select or exclude packages or ranges of packages with the following syntax:

`Enter a selection (default=all): 1-10 15`

which will select packages 1 through 10 and 15 for installation, or:

`Enter a selection (default=all): ^5-8 ^2`

which will select all packages except 5 through 8 and 2 for installation.

To see what packages belong to the gnome group, run:

`$ pacman -Sg gnome`

Also visit [https://archlinux.org/groups/](https://archlinux.org/groups/) to see what package groups are available.

**Note:** If a package in the list is already installed on the system, it will be reinstalled even if it is already up-to-date. This behavior can be overridden with the `--needed` option.

### Removing packages

To remove a single package, leaving all of its dependencies installed:

`# pacman -R _package_name_`

To remove a package and its dependencies which are not required by any other installed package:

`# pacman -Rs _package_name_`

**Warning:** When removing a group, such as _gnome_, this ignores the install reason of the packages in the group, because it acts as though each package in the group is listed separately. Install reason of dependencies is still respected.

The above may sometimes refuse to run when removing a group which contains otherwise needed packages. In this case try:

`# pacman -Rsu _package_name_`

To remove a package, its dependencies and all the packages that depend on the target package:

**Warning:** This operation is recursive, and must be used with care since it can remove many potentially needed packages.

`# pacman -Rsc _package_name_`

To remove a package, which is required by another package, without removing the dependent package:

**Warning:** The following operation can break a system and should be avoided. See [System maintenance#Avoid certain pacman commands](https://wiki.archlinux.org/title/System_maintenance#Avoid_certain_pacman_commands "System maintenance").

`# pacman -Rdd _package_name_`

_Pacman_ saves important configuration files when removing certain applications and names them with the extension: _.pacsave_. To prevent the creation of these backup files use the `-n` option:

`# pacman -Rn _package_name_`

**Note:** _Pacman_ will not remove configurations that the application itself creates (for example "dotfiles" in the home directory).

### Upgrading packages

**Warning:**
- Users are expected to follow the guidance in the [System maintenance#Upgrading the system](https://wiki.archlinux.org/title/System_maintenance#Upgrading_the_system "System maintenance") section to upgrade their systems regularly and not blindly run the following command.
- Arch only supports full system upgrades. See [System maintenance#Partial upgrades are unsupported](https://wiki.archlinux.org/title/System_maintenance#Partial_upgrades_are_unsupported "System maintenance") and [#Installing packages](https://wiki.archlinux.org/title/Pacman#Installing_packages) for details.

- _Pacman_ can update all packages on the system with just one command. 
	- This could take quite a while depending on how up-to-date the system is. 
- The following command synchronizes the repository databases _and_ updates the system's packages, excluding "local" packages that are not in the configured repositories:

`# pacman -Syu`

### Querying package databases

_Pacman_ queries the local package database with the `-Q` flag, the sync database with the `-S` flag and the files database with the `-F` flag. See `pacman -Q --help`, `pacman -S --help` and `pacman -F --help` for the respective suboptions of each flag.

_Pacman_ can search for packages in the database, searching both in packages' names and descriptions:

`$ pacman -Ss _string1_ _string2_ ...`

Sometimes, `-s`'s builtin ERE (Extended Regular Expressions) can cause a lot of unwanted results, so it has to be limited to match the package name only; not the description nor any other field:

`$ pacman -Ss '^vim-'`

To search for already installed packages:

`$ pacman -Qs _string1_ _string2_ ...`

To search for package file names in remote packages:

`$ pacman -F _string1_ _string2_ ...`

To display extensive information about a given package:

`$ pacman -Si _package_name_`

For locally installed packages:

`$ pacman -Qi _package_name_`

Passing two `-i` flags will also display the list of backup files and their modification states:

`$ pacman -Qii _package_name_`

To retrieve a list of the files installed by a package:

`$ pacman -Ql _package_name_`

To retrieve a list of the files installed by a remote package:

`$ pacman -Fl _package_name_`

To verify the presence of the files installed by a package:

`$ pacman -Qk _package_name_`

Passing the `k` flag twice will perform a more thorough check.

To query the database to know which package a file in the file system belongs to:

`$ pacman -Qo _/path/to/file_name_`

To query the database to know which remote package a file belongs to:

`$ pacman -F _/path/to/file_name_`

To list all packages no longer required as dependencies (orphans):

`$ pacman -Qdt`

To list all packages explicitly installed and not required as dependencies:

`$ pacman -Qet`

See [pacman/Tips and tricks](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks "Pacman/Tips and tricks") for more examples.

#### Pactree

**Note:** [pactree(8)](https://man.archlinux.org/man/pactree.8) is not part of the [pacman](https://archlinux.org/packages/?name=pacman) package anymore. Instead it can be found in [pacman-contrib](https://archlinux.org/packages/?name=pacman-contrib).

To view the dependency tree of a package:

$ pactree _package_name_

To view the dependant tree of a package, pass the reverse flag `-r` to _pactree_.

#### Database structure

- The _pacman_ databases are normally located at `/var/lib/pacman/sync`. 
- For each repository specified in `/etc/pacman.conf`, there will be a corresponding database file located there. 
- Database files are gzipped tar archives containing one directory for each package, for example for the [which](https://archlinux.org/packages/?name=which) package:

```
$ tree which-2.21-5

which-2.21-5
|-- desc
```

The `desc` file contains meta data such as the package description, dependencies, file size and MD5 hash.

### Cleaning the package cache

_Pacman_ stores its downloaded packages in `/var/cache/pacman/pkg/` and does not remove the old or uninstalled versions automatically. This has some advantages:

1. It allows to [downgrade](https://wiki.archlinux.org/title/Downgrade "Downgrade") a package without the need to retrieve the previous version through other means, such as the [Arch Linux Archive](https://wiki.archlinux.org/title/Arch_Linux_Archive "Arch Linux Archive").
2. A package that has been uninstalled can easily be reinstalled directly from the cache directory, not requiring a new download from the repository.

However, it is necessary to deliberately clean up the cache periodically to prevent the directory to grow indefinitely in size.

The [paccache(8)](https://man.archlinux.org/man/paccache.8) script, provided within the [pacman-contrib](https://archlinux.org/packages/?name=pacman-contrib) package, deletes all cached versions of installed and uninstalled packages, except for the most recent three, by default:

`# paccache -r`

[Enable](https://wiki.archlinux.org/title/Enable "Enable") and [start](https://wiki.archlinux.org/title/Start "Start") `paccache.timer` to discard unused packages weekly.

**Tip:** You can create a [hook](https://wiki.archlinux.org/title/Pacman#Hooks) to run this automatically after every _pacman_ transaction, [install](https://wiki.archlinux.org/title/Install "Install") [paccache-hook](https://aur.archlinux.org/packages/paccache-hook/)AUR and see [other examples](https://bbs.archlinux.org/viewtopic.php?pid=1694743#p1694743)

You can also define how many recent versions you want to keep. To retain only one past version use:

`# paccache -rk1`

Add the `-u`/`--uninstalled` switch to limit the action of _paccache_ to uninstalled packages. For example to remove all cached versions of uninstalled packages, use the following:

`# paccache -ruk0`

See `paccache -h` for more options.

_Pacman_ also has some built-in options to clean the cache and the leftover database files from repositories which are no longer listed in the configuration file `/etc/pacman.conf`. However _pacman_ does not offer the possibility to keep a number of past versions and is therefore more aggressive than _paccache_ default options.

To remove all the cached packages that are not currently installed, and the unused sync databases, execute:

`# pacman -Sc`

To remove all files from the cache, use the clean switch twice, this is the most aggressive approach and will leave nothing in the cache directory:

`# pacman -Scc`

**Warning:** One should avoid deleting from the cache all past versions of installed packages and all uninstalled packages unless one desperately needs to free some disk space. This will prevent downgrading or reinstalling packages without downloading them again.

[pkgcacheclean](https://aur.archlinux.org/packages/pkgcacheclean/)AUR and [pacleaner](https://aur.archlinux.org/packages/pacleaner/)AUR are two further alternatives to clean the cache

### Additional commands

Download a package without installing it:

`# pacman -Sw _package_name_`

Install a 'local' package that is not from a remote repository (e.g. the package is from the [AUR](https://wiki.archlinux.org/title/AUR "AUR")):

`# pacman -U _/path/to/package/package_name-version.pkg.tar.zst_`

To keep a copy of the local package in _pacman'_s cache, use:

`# pacman -U file:///_path/to/package/package_name-version.pkg.tar.zst_`

Install a 'remote' package (not from a repository stated in _pacman'_s configuration files):

`# pacman -U _http://www.example.com/repo/example.pkg.tar.zst_`

### Installation reason

The _pacman_ database organizes installed packages into two groups, according to installation reason:

- **explicitly-installed**: packages that were literally passed to a generic _pacman_ `-S` or `-U` command;
- **dependencies**: packages that, despite never (in general) having been passed to a _pacman_ installation command, were _implicitly_ installed because they were [required](https://wiki.archlinux.org/title/Dependency "Dependency") by packages explicitly installed.

When installing a package, it is possible to force its installation reason to _dependency_ with:

`# pacman -S --asdeps _package_name_`

The command is normally used because explicitly-installed packages may offer [optional packages](https://wiki.archlinux.org/title/Optdepends "Optdepends"), usually for non-essential features for which the user has discretion.

**Tip:** Installing optional dependencies with `--asdeps` will ensure that, if you [remove orphans](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks#Removing_unused_packages_(orphans) "Pacman/Tips and tricks"), _pacman_ will also remove optional packages set this way.

When **re**installing a package, though, the current installation reason is preserved by default.

The list of explicitly-installed packages can be shown with `pacman -Qe`, while the complementary list of dependencies can be shown with `pacman -Qd`.

To change the installation reason of an already installed package, execute:

`# pacman -D --asdeps _package_name_`

Use `--asexplicit` to do the opposite operation.

**Note:** Using `--asdeps` and `--asexplicit` options when upgrading, such as with `pacman -Syu _package_name_ --asdeps`, is discouraged. This would change the installation reason of not only the package being installed, but also the packages being upgraded.