- <mark style="background: #ABF7F7A6;">FTP</mark>, file transfer protocol, is a popular method of transferring files to and from remote servers
	- deprecated in 2022 due to lack of security features
- <mark style="background: #ABF7F7A6;">SFTP</mark>, the secure (or SSH) File Transfer Protocol, provides functionality similar to FTP, but uses SSH for security 
- you can connect to your remote server using the sftp utility
- the command for this is almost the same as ssh

```bash
ssh -i ~/.ssh/do-key user@ip-address
sftp -i ~/.ssh/do-key user@ip-address
```

If you have a host configured in your `~/.ssh/config` file, like the example below.

```bash
Host arch
  HostName 146.190.141.176
  User pond
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/do-key
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```

you can use this connect to your server via sftp too

```bash
`sftp arch`
```

Once you are connect you will see the sftp prompt

```
sftp>
```

type `?` or `help` and press enter to see the help menu for sftp

Some commands have a local and remote option. For example

```
sftp> lmkdir dir-name # this will create a local directory
sftp> mkdir dir-name # this will create a remote directory
```

- **get** will download a file. The file will be downloaded to the directory where you run the sftp command to make the connection to your server

```
sftp> get file-path
```

- **put** will upload a file. By default you connect to your users home directory, so the file will be uploaded to the users home directory.

```
sftp> put file-path
```

- You can also upload and download directories.

```
sftp> put -r directory
```

- **exit** will exit out of the current process environment. Just like it does with ssh.

```
sftp> exit
```

### Midterm reflection
- know what ps -eo comm,rss | grep -i bash 
- "var = val" does not work in bash because bash will assume var is a command name and try to run it 
	-  `var` - Bash assumes this is a command name.
	- `=`  Bash interprets this as an argument to the `var` command.
	- `val`  Bash interprets this as another argument to the `var` command.
- use `mv` to move things and also can use it to rename them 
- `bashrc` file for aliases 
- what id PID1 = initializes process (init) (system D)
- [0-9]{2} - means to look for this pattern twice. matches to a 2 digit long
	- the {2} tells you how many times to look 
- `?` in case for any single input 

**Reference:**

[Linuxize SFTP](https://linuxize.com/post/how-to-use-linux-sftp-command-to-transfer-files/)