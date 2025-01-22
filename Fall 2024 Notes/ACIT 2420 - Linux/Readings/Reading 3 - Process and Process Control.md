### Executables versus processes
- programs are distributed executable files
- later systems invented special formats for executable files

#### Executable and Linkable Format (ELF)
- <mark style="background: #ABF7F7A6;">Executable and Linkable Format</mark> (ELF)
	- the default executable file format on Linux and other Unix operating systems
	- ELF file stores executable code of programs
		- it can also store debug information 
	- ELF files can also be linked with other ELF files, known as <mark style="background: #ABF7F7A6;">shared libraries</mark> 
		- files that contain executable code but aren't meant to run as programs and only serve as collections of reusable functions 
- ELF stores metadata such as the target OS and CPU architecture
- Linux kernel knows how to load ELF files

#### ld linux.so
- The role of <mark style="background: #ABF7F7A6;">ld-linux.so</mark> is not to interpret the executable itself but, instead, to correctly resolve references to functions that come from shared libraries.
- If you run **file** on it, you will see that it’s **static-pie linked** rather than **dynamically linked**, unlike **/bin/bash** — **static-pie** means **static** **position-independent executable**:
- the kernel knows nothing about programs' library function dependencies and can only load statically linked ELF executables directly
- to load dynamically linked executables, it relies on the `ld-linux.so` helper but reuses a general interpreter association mechanism for it 
- once an executable file is loaded, directly by the kernel itself or with help from an interpreter, it becomes a running *process* 

### Process Termination and Exit Codes

#### Exit Codes
- every process terminates with an <mark style="background: #ABF7F7A6;">exit code</mark>
	- a numeric value that indicates whether it exited normally or terminated due to an error
- a zero exit code means success
	- any non zero exit code indicates an error
	- no standard meaning for non zero exit codes
		- exact meanings will vary between programs and OS 
- many programs will exit with 1 for any error
- In bash, you can find the exit code of the last command in a special variable named **$?**
- usually programs set their exit codes themselves
- For programs that can be executed non-interactively from scripts, it’s critically important to exit with a non-zero code on errors; 
	- otherwise, script authors will have no way to know whether their script steps succeeded or failed.

example of exit code
```bash
$ touch /etc/my_file
touch: cannot touch '/etc/my_file': Permission denied
$ echo $?
1
```

- The simplest use case for exit codes is chaining commands with the **||** and **&&** operators.
- On errors such as the file permission error in our example, the kernel simply does not do what the program asks it to do and, instead, allows the program to continue running as usual.
	- The reasoning is that such errors often occur due to incorrect user input while the program logic is correct, so the program needs to be able to handle them and notify the user.

#### Signals
-  a signal is a special condition that may occur during process execution 
- **SIGILL** — _illegal instruction_ (caused, for example, by attempts to divide by zero)
-  **SIGSEV** — _segmentation violation_ (caused by trying to read or modify memory that wasn’t allocated to the process)
- other signals are generated on external conditions to force a process to handle them
- **SIGPIPE** is generated when a network socket or a local pipe is closed by the other end 
- **SIGINT** (which interrupts a process)
-  **SIGTERM** (which asks the process to clean up its state and terminate)
- **SIGKILL** (which tells the kernel to forcibly terminate a process)
- the kernel that has execution control when a signal is generated, not the process.
- Programmers may anticipate certain signals and register _signal handlers_ for them.

#### The Kill Command
- <mark style="background: #ABF7F7A6;">kill</mark> is the command for sending signals to process
	- it us usually used to forcibly terminate processes, but it can also send other signals as well
- When you press _Ctrl_ + _C_ to terminate a process in the shell, you actually ask your shell to send it a **SIGINT** signal – a signal to interrupt execution.
- If a process is in the background, we cannot use _Ctrl_ + _C_ to interrupt it. However, with **kill**, we can send it manually
- By default, the **kill** command sends a **SIGTERM** signal. 
- Both **SIGINT** and **SIGTERM** can be caught or ignored by a process.
	- by sending them to a process, you ask it to terminate; a well behaved process should comply and may use it as a chance to finalize its current task before terminating 

#### The Process Tree
- everything you launch from your shell becomes a _child process_ of that shell process.
- the shell itself is a child process
	- either of your terminal emulator if you are on a Linux desktop, or of the OpenSSH daemon if you connect remotely over SSH.
- there is a parent of all processes, and all running process relationships form a tree with a single root (PID =1)
- the PID = 1 process can be anything 
- when you boot a Linux system, you can tell is which executable to load as PID = 1
- For example, one way to boot a system in rescue mode is to append **init=/bin/bash** to the GRUB command line (but you are better off using a built-in rescue option in your distro’s boot menu item because it may pass additional useful parameters)
- The **init** process is the only process that is launched directly by the kernel. 
- Everything else is launched by the **init** process instead: login managers, the SSH daemon, web servers, database systems – everything you can think of. 
- You can view the full process tree with the **pstree** command.

### Process Search and Monitoring
-  the <mark style="background: #ABF7F7A6;">pstree</mark> command is a great way to visualize all running processes and relationships between them 
	- but in practice, most of the time admins look for specific processes or need to learn about their resource usage rather than their mere existence 
- the `ps` command to search processes
- the `top` command to monitor resource usage in real time, and the underlying kernel interface that those tools use 

#### The ps command
- PS is an abbreviation for process selection or process snapshot
- it is a utility that allows you to retrieve and filter information about running processes
- Running **ps** without any arguments will get you a very limited selection – only processes that run from your user and that are attached to a _terminal_
	- ps will always show up in such lists because when it gathers that info, it is a running process
- ![[Pasted image 20240923215856.png]]
- the `PID` field is a process identifier
	-  a unique number that the kernel assigns to every process when the process is launched 
	- if present, the TTY field is a terminal
		- it can be a real serial console (usually **ttyS*** or **ttyUSB***), a virtual console on a physical display (**tty***), or a purely virtual pseudo-terminal associated with an SSH connection or a terminal emulator (**pts/***).
- the `CMD` field shows the command that was used to launch a process with its arguments, if any were used 
- The **ps** command has a large number of options. Two options you should learn about right away are **a** and **x** — options that remove _owned by me_ and _have a_ _terminal_ restrictions.
- A common command to view every process on the system is **ps ax**.
- ![[Pasted image 20240923221731.png]]
- The **STAT** field tells us the process state. 
- The **S** state means a process is in the interruptible sleep state – it waits for external events. 
- Processes that currently do something can be seen in the **R** state – running. 
- The **I** state is special; it is for idle kernel threads. 
- The most concerning state is **D** – uninterruptible sleep. 
	- processes are in the uninterruptible sleep state if they actively wait for input/output operations to complete, so if there is a large number of such processes, it may mean that the input/output systems are overloaded.
- processes with command names in square brackets that you don’t see in the output of **pstree**. 
	- Those are, in fact, kernel services that are made to look like processes for ease of monitoring, or for their own internal reasons.

#### Process Monitoring Tools
- If you want to find out what causes CPU usage spikes, that’s very inconvenient – you need to be lucky to run **ps** exactly when a spike occurs. That’s why people wrote tools that monitor processes continuously and can display them sorted by resource consumption.
- One of the oldest tools in this class is called <mark style="background: #ABF7F7A6;">top</mark>
	- it displays an interactive process list, with processes that consume the largest resources automatically floating to the top
- There are other tools inspired by it, such as **htop**, which offers different user interfaces and additional functionality. 
- There are also tools that monitor resource usage types that **top** or **htop** don’t, such as **iotop**, a tool to monitor the input/output activity of processes.

#### The /proc filesystem
- the lowest-level interface to process information – the **/proc** filesystem.
	- an example of a virtual filesystem 
- to users, /proc looks like a dictionary with files
	- some files having nothing to do with processes and contain other system info instead 
- it’s good to know about the raw **/proc** interface, but it’s impractical to use it as a source of process information. Tools such as **ps** and **pstree** can present it in a much more readable form and allow you to filter it.
- it’s also important to understand that those tools don’t use any special kernel interfaces other than **/proc**, and in an emergency situation, you can always get the same information from there without any tools at all.