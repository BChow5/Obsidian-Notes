### Bash Variables
- bash can create variables to store info
- The name of a variable must start with a letter but can contain any sequence of letters or numbers. Except for the underscore "_", special characters cannot be used.
- `variable=value`
- recommended to protect variables with quotes
	- ' will inhibit interpretation of special characters
- `unset` to delete variable
- `readonly` or `typeset -r` locks a variable

#### Environment variables
- environment variables and system variables are variables used by the system for its operation
- `env` displays all environment variables
- `set` displays all system variables

|Variables|Description|
|---|---|
|`HOSTNAME`|Host name of the machine.|
|`USER`, `USERNAME` and `LOGNAME`|Name of the user connected to the session.|
|`PATH`|Path to find the commands.|
|`PWD`|Current directory, updated each time the cd command is executed.|
|`HOME`|Login directory.|
|`$$`|Process id of the script execution.|
|`$?`|Return code of the last command executed.|
- `export` to export a variable
- in order for the child processes of the script to know the variables, they must be exported

#### Substitute
- `variable=command
variable=$(command) # Preferred syntax`


### Bash Heredoc
- heredoc is a type of redirection that allows you to pass multiple lines of input to a command

```
[COMMAND] <<[-] 'DELIMITER'
  HERE-DOCUMENT
DELIMITER
```
- the first line starts with an optional command followed by the special redirection operator `<<` and the delimiting identifier.
- You can use any string as a delimiting identifier, the most commonly used are EOF or END.
- If the delimiting identifier is unquoted, the shell will substitute all variables, commands and special characters before passing the here-document lines to the command.
- Appending a minus sign to the redirection operator `<<-`, will cause all leading tab characters to be ignored. This allows you to use indentation when writing here-documents in shell scripts. Leading whitespace characters are not allowed, only tab.
-  The here-document block can contain strings, variables, commands and any other type of input.
- The last line ends with the delimiting identifier. White space in front of the delimiter is not allowed.
- when the delimiter is quoted no parameter expansion and command substitution is done by the shell.

you can redirect it to a file using the `>`, `>>` operators.

```
cat << EOF > file.txt
The current working directory is: $PWD
You are logged in as: $(whoami)
EOF
```
- If the file.txt doesn’t exist it will be created. When using `>` the file will be overwritten, while the `>>` will append the output to the file.

#### Heredoc with SSH
- easy way to execute commands on a remote system over SSH

``` bash
ssh -T user@host.com << EOF
echo "The current local working directory is: $PWD"
echo "The current remote working directory is: \$PWD"
EOF
```


### Systemctl commands
- systemctl is a controlling interface and inspection tool for the init system and service manager systemd

#### managing services
- systemd initializes user space componenents that run after the linux kernel has booted and maintains those componenets
- these tasks are known as units
	- each unit has a corresponding unit file
	- Units might concern mounting storage devices (.mount), configuring hardware (.device), sockets (.socket), or, as will be covered in this guide, managing services (.service)

#### start and stop service
- `sudo systemctl start apache2.service`
- `sudo systemctl stop apache2.service`
- if restart not needed, you can use `reload`
- can use `reload-or-restart` if unsure which is needed

#### enable service at boot
- `sudo systemctl enable nginx`
- `sudo systemctl disable nginx`

#### check status
- `systemctl status mysql`
- You can also use `is-active`, `is-enabled`, and `is-failed` to monitor a service’s status:

To view which `systemd` service units are currently active on your system, issue the following `list-units` command and filter by the service type:

```
systemctl list-units --type=service
```

#### working with unit files
- Each unit has a corresponding _unit file_. These unit files are usually located in the following directories:
- The `/lib/systemd/system` directory holds unit files that are provided by the system or are supplied by installed packages. This directory is also a symlink to `/usr/lib/systemd/user/` directory.
- The `/etc/systemd/system` directory stores unit files that are user-provided.

#### view dependencies for unit file
- To display a list of a unit file’s dependencies, use the `list-dependencies` command:

```
systemctl list-dependencies cron
```

#### editing unit file

There are two ways to edit a unit file using `systemctl`.

1. The `edit` command opens up a blank drop-in snippet file in the system’s default text editor:
    
    ```
    sudo systemctl edit ssh
    ```
    
    When the file is saved, `systemctl` will create a file called `override.conf` under a directory at `/etc/systemd/system/yourservice.service.d`, where `yourservice` is the name of the service you chose to edit. This command is useful for changing a few properties of the unit file.
    
2. The second way is to use the `edit` command with the `--full` flag:
    
    ```
    sudo systemctl edit ssh --full
    ```

#### Create Unit file
While `systemctl` will throw an error if you try to open a unit file that does not exist, you can force `systemctl` to create a new unit file using the `--force` flag:

```
sudo systemctl edit yourservice.service --force
```

#### Removing a Unit File[](https://www.linode.com/docs/guides/introduction-to-systemctl/#removing-a-unit-file)

To remove a unit file snippet that was created with the `edit` command, remove the directory `yourservice.service.d` (where ‘yourservice’ is the service you would like to delete), and the `override.conf` file inside of the directory:

```
sudo rm -r /etc/systemd/system/yourservice.service.d
```

To remove a full unit file, run the following command:

```
sudo rm /etc/systemd/system/yourservice.service
```

After you issue these commands, reload the `systemd` daemon so that it no longer tries to reference the deleted service:

```
sudo systemctl daemon-reload
```