### Navigating in the shell

We are going to look at the Linux filesystem more next week. Today we are just going to be working in our home directory.

A users home directory is `/home/user-name`. Two shortcuts for this are:

- `~` the tilde
- `$HOME` the home path environment variable

Generally when you open up your terminal or SSH into a server you will start in the users home directory.

The prompt, the text in your shell generally includes your location by default. You can also see where you are with the `pwd` command.

### What is a shell?

Since we are going to use the shell a lot we are going to talk about it a lot as well.

Last week I mentioned that we would be using the [Bash](https://www.gnu.org/software/bash/) shell. This is the most common shell and runs as the default on most Linux distros.

In general the shell is a tool that allows you to execute commands run by your operating system. These can be interactive commands, or commands saved as file in a shell script.

The shell functions as a REPL

- Read
- Evaluate
- Print
- Loop

When you run a command the shell looks for that command in several locations

- Shell built-ins
- Shell functions
- Commands in your file path

### Shell built-ins

Run the command `help` to get a list of shell built-ins. We are just going to look at two today.

- `cd`
- `echo`

### Commands in your path
#VimCommands 
You can print your path with the command `echo $PATH` this will show you a list of all of the locations that your shell will look for commands when you run a command.

Create a directory with `mkdir` You can create nested directories with the `-p` option. For example

```
mkdir -p one/two/three
```

removing a directory. The reading recommends the `rmdir` command. However since this only removes empty directories I never use this. Instead I always use `rm` with the `-r` option.

The `ls` command to list the contents of a directory has a few handy options.

- `-l` long list ing
- `-a` all
- `-F` classify this appends an indicator that tells you what a file is, for example directories.

> [!tip] Some helpful command line tips
> 
> - Press "tab" to complete commands
> - "ctrl+r" will show you recent commands in your shells history
> - "ctrl+l" or the command "clear" will clear the screen
> - "ctrl+e" will take you to the end of a command
> - "ctrl+a" will take you to the start of a command
> - "ctrl+u" will delete text from the cursor to the prompt

## Intro to working with Vim (or Neovim)

Vim was created by Bram Moolenaar in 1991 and was based on vi (pronounced v.i.). Neovim was first released in 2005 and is a community re-write of Vim.

Vim is what is known as a modal text editor. It has different modes for performing different tasks. Each mode has specific keybindings intended to make working in that mode as efficient as possible.

The Major modes are:

- Normal (the default mode where you do most editing tasks)
- Insert (where you enter text)
- Visual (where you can select text visually)
- Command Line (where you enter commands that begin with `:`)

### Open a file in Vim

`nvim file-name-path` eg `nvim ~/.bashrc`

### Edit a file in Vim
#VimCommands 
In "Normal" mode, the default mode we can perform tasks like deleting lines of text, finding a specific part of a file and pasting text. When you first open Vim you are in Normal mode.

To enter "Insert" mode press `i`.

To return to Normal mode press `escape`

To save a file use "Command" mode `:w`

To quit Vim use Command mode `:q`

Save and quit can be combined `:wq`

If you want to quit without saving `:q!`

> [!warning] Ctrl + z Pressing `Ctrl+z` doesn't quit vim. `Ctrl+z` makes a running process a background process. Use `:q` to quit vim

> [!note] `nvim` command Because we installed [Neovim](https://neovim.io/) instead of Vim the command we use to open the editor is `nvim`. If we had installed Vim the command would be `vim`. For the Vim content covered in class Vim and Neovim will be almost identical, I will include a note where there is a difference.