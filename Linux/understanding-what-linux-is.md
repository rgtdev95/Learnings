## Understanding What Linux Is

### What is Linux?

Linux is a free and open-source operating system (OS) kernel. It is a Unix-like operating system that is based on the Linux kernel, which is a free and open-source software project that is developed by a community of volunteers. Linux is a popular choice for servers, supercomputers, and embedded systems.

### Learning where Linux came from

Linux was created by Linus Torvalds in 1991. It was initially developed as a personal project to replace the Minix operating system, which was used in his university courses. Torvalds released the Linux kernel as free software in 1992, and the Linux community has since grown to include a large number of developers and users.

### Choosing Linux Distributions

There are many different Linux distributions available, each with its own unique features and capabilities. Some popular Linux distributions include Ubuntu, Fedora, Debian, and Arch Linux.



# Things that make up the Linux system

- Detecting and preparing the hardware
- Managing Processes
- Managing Memory
- Providing User Interface
- Controlling File System
- Providing user access and authentication
- Offer Administrative Utilities
- Starting up services
- Progamming Tools


# Advance features in Linux
- Clustering
- Virtualization
- Containerization
- Cloud Computing
- Real-time computing
- Specialized storage


### How linux Differs from other Operating systems

- Open Source
- Free
- Customizable
- Secure
- Stable

### Linux History

- Unix was created by Bell Labs in 1969
- Unix was created by Dennis Ritchie and Ken Thompson
- 1991 - Linux kernel is released
- Clone from Unix
- Commercial Unix
    - BSD Berkeley Software Distribution
    - Bell Labs version of Unix


# what is shell?

A Linux shell is a command-line interface (CLI) that lets you interact with the Linux operating system by typing commands instead of clicking buttons.

Think of it as a translator between you and Linux.

- CLI means Command Line Interface / Interpreter

# ~ for regular user
$ for root user

# Types of Shell
- Bash
- Zsh
- Fish

# difference between them

- **Bash (Bourne Again SHell)**: The most common default shell for Linux distributions. It is POSIX-compliant, highly stable, and the standard for shell scripting.

- **Zsh (Z Shell)**: An extension of Bash with many improvements. It offers advanced tab completion, spelling correction, and high customizability through frameworks like "Oh My Zsh." It is the default shell for macOS.

- **Fish (Friendly Interactive SHell)**: Designed for user-friendliness and out-of-the-box functionality. It features syntax highlighting, auto-suggestions as you type, and a web-based configuration tool. Note that it is not fully POSIX-compliant.



# Running Commands
- Type the command
- Press Enter

# Command Syntax
- Command [Options] [Arguments]

# Options
- -h or --help
- -v or --version
- -q or --quiet 

# Arguments
- Files
- Directories
- URLs
- IP addresses
- Ports
- etc

# What is 'id' command?
- id command is used to display user and group information.

# where the commands are located by default in linux?
- /bin
- /usr/bin
- /usr/local/bin

# how to find the location of a command?
- which command
- whereis command       

# order in which the shell checks for commands user type:
- Aliases
- Shell resevered word
- Functions
- Built-in commands
- PATH or file system commands

# How Commands Work
- When you type a command, the shell searches for it in the directories listed in your `$PATH` environment variable.
- If found, the shell executes the corresponding binary or script.
- Executing a program is essentially running a command; the command name is often just the name of the executable file.
- If the program is not in your `$PATH`, you must provide the full or relative path (e.g., `./myprogram` or `/opt/app/bin/start`).

what is purpose of $PATH?
- $PATH is a list of directories that the shell searches for commands when you type a command name.
- It is a colon-separated list of directories, with each directory separated by a colon (e.g., `/bin:/usr/bin:/usr/local/bin`).
- The shell searches for commands in the order of the directories in the `$PATH` list.
- If the command is not found in any of the directories in the `$PATH`, the shell returns an error.

- use command: history to view all previous commands

# Command Line Navigation Shortcuts
- **Ctrl + A**: Move cursor to the beginning of the line.
- **Ctrl + E**: Move cursor to the end of the line.
- **Ctrl + U**: Delete from the cursor to the beginning of the line.
- **Ctrl + K**: Delete from the cursor to the end of the line.
- **Ctrl + W**: Delete the word before the cursor.
- **Ctrl + L**: Clear the terminal screen.
- **Ctrl + C**: Interrupt/Abort the current command.
- **Alt + B**: Move cursor backward by one word.
- **Alt + F**: Move cursor forward by one word.
- **Tab**: Auto-complete commands, file names, or paths.

# Command Line Editing & History Shortcuts
- **Ctrl + Y**: Paste (yank) the last deleted text.
- **Ctrl + _**: Undo the last editing command.
- **Ctrl + T**: Swap the character before the cursor with the one under the cursor.
- **Alt + T**: Swap the current word with the previous word.
- **Alt + U**: Convert the word from the cursor to the end to uppercase.
- **Alt + L**: Convert the word from the cursor to the end to lowercase.
- **Alt + C**: Capitalize the current word.
- **Ctrl + R**: Search backward through command history.
- **Ctrl + P**: Move to the previous command in history.
- **Ctrl + N**: Move to the next command in history.

# Cutting and Pasting Shortcuts
- **Alt + D**: Cut the word after the cursor.
- **Alt + Y**: Cycle through the kill ring of previously cut text (only works after pressing Ctrl + Y).
- **Ctrl + W**: Cut the word before the cursor (adds to clipboard/kill-ring).
- **Ctrl + K**: Cut text from cursor to end of line (adds to clipboard/kill-ring).
- **Ctrl + U**: Cut text from cursor to beginning of line (adds to clipboard/kill-ring).
- **Ctrl + Y**: Paste the last cut text at the cursor position.


# Using Command History
- **Up Arrow**: Go to the previous command.
- **Down Arrow**: Go to the next command.
- **!!**: Run the last command again.
- **!n**: Run the nth command in history.
- **!-n**: Run the command n lines back in history.
- **!string**: Run the most recent command starting with 'string'.
- **!?string?**: Run the most recent command containing 'string'.
- **Ctrl + R**: Incremental search backward through history.
- **Ctrl + S**: Incremental search forward through history.
- **Alt + P**: Non-incremental search backward through history.
- **Alt + N**: Non-incremental search forward through history.

shell history is stored in .bash_history file
Note: some people disable history in bash for root user

# Connecting and Expanding Commands

Meta characters: 
- * : matches any number of characters
- ? : matches any single character
- [ ] : matches any character in the set
- [^ ] : matches any character not in the set
- ( ) : group
- | : or
- + : one or more
- ^ : start of line
- $ : end of line

### Examples of Meta Characters

- `ls *.txt`: Matches all files with a `.txt` extension.
- `ls file?.txt`: Matches `file1.txt`, `fileA.txt`, but not `file10.txt`.
- `ls [0-9]*`: Matches any file starting with a digit.
- `ls [^a-z]*`: Matches any file not starting with a lowercase letter.
- `grep -E "(error|warning)" logs.txt`: Groups patterns to search for either term.
- `ls | grep "config"`: Passes the output of `ls` to `grep` to filter for "config".
- `grep -E "lo+g" file.txt`: Matches "log", "loog", "looog", etc. (one or more 'o').
- `grep "^Title" doc.txt`: Matches lines starting with "Title".
- `grep "EOF$" doc.txt`: Matches lines ending with "EOF".


### Piping between commands
- `ls | grep "config"`

### Sequential Commands
- `ls; grep "config"`

### Background Commands
- `ls &`

### Expanding commands
- `ls`

### Expanding arithmetic expressions
- `expr 1 + 2`

### Expanding variables
- `echo $PATH`

### Expanding wildcards
- `ls *.txt`

### Expanding variables
- `echo $PATH`

### Expanding wildcards
- `ls *.txt`


# Using Shell Variables
The shell itself stores information that may be useful to the user's shell session in waht are called variables.

### Examples of Shell Variables
- `echo $PATH`
- `echo $HOME`
- `echo $USER`
- `echo $SHELL`
- `echo $PWD`
- `echo $OLDPWD`
- `echo $LANG`
- `echo $LANG`

### Common Environment Variables
- **HOME**: The current user's home directory.
- **PATH**: A list of directories to search for executable commands.
- **USER**: The name of the current logged-in user.
- **SHELL**: The path to the current user's shell program.
- **PWD**: The current working directory.
- **OLDPWD**: The previous working directory.
- **TERM**: The type of terminal being used.
- **EDITOR**: The default text editor used by commands.
- **HOSTNAME**: The name of the computer on the network.
- **LANG**: The current system locale and language settings.
- **UID**: The numeric user ID of the current user.


# Bash Configuration Files
- **/etc/profile**: System-wide environment and startup programs for login shells.
- **~/.bash_profile**: User-specific environment and startup programs for login shells.
- **~/.bashrc**: User-specific aliases and functions for interactive non-login shells.
- **/etc/bashrc**: System-wide aliases and functions for interactive shells.
- **~/.bash_logout**: User-specific cleanup script executed when a login shell exits.
- **~/.inputrc**: Configuration for the Readline library (command-line editing behavior).

# The Shell Prompt
The shell prompt is the visual indicator that the shell is ready to accept a command. It provides essential context about the current session and environment.

### Prompt Variables
- **PS1**: The primary prompt string displayed before each command.
- **PS2**: The secondary prompt displayed for multi-line commands (default: `>`).
- **PS3**: The prompt used for the `select` loop command.
- **PS4**: The prompt used for execution tracing (debugging).

### Common PS1 Escape Sequences
- `\u`: Current username.
- `\h`: Hostname of the machine.
- `\w`: Current working directory (full path).
- `\W`: Basename of the current working directory.
- `\$`: Displays `#` if the user is root, otherwise `$`.
- `\t`: Current time in HH:MM:SS format.

### Example Customization
To change the prompt, export the `PS1` variable in `~/.bashrc`:
```bash
export PS1="[\u@\h \W]\$ "
```

# Add/Remove Environment Variables
To define a new environment variable for the current session, use the `export` command:

```bash
export MY_VAR="hello_world"
```

To make a variable persistent across sessions, add the export statement to your `~/.bashrc` or `~/.bash_profile` file:

```bash
echo 'export MY_VAR="hello_world"' >> ~/.bashrc
source ~/.bashrc
```

### Removing Environment Variables
To remove a variable from the current session, use the `unset` command:

```bash
unset MY_VAR
```

To permanently remove a variable, delete the corresponding `export` line from your configuration file and restart the shell or source the file again.

# Getting information about commands

ls /bin

echo $PATH

help command
-- help with command
info command
man command