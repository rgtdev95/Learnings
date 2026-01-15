# Running Commands

## Key Concepts

| Term | Definition |
|------|------------|
| **Command** | An instruction to the shell to execute a program |
| **Options** | Modifiers that change command behavior (e.g., `-l`, `--help`) |
| **Arguments** | Inputs the command operates on (files, directories, text) |
| **Exit Status** | Numeric value indicating success (0) or failure (non-zero) |
| **PATH** | Environment variable listing directories to search for commands |

---

## Running Basic Commands

The simplest way to run a command is to type its name. The book suggests starting with these:

```bash
# Display current date and time
date
# Output: Thu Jun 29 08:14:53 EDT 2019

# Show current working directory
pwd
# Output: /home/chris

# Show computer's hostname
hostname
# Output: mydesktop

# List files in current directory
ls
# Output: Desktop Documents Downloads Music Pictures Public Templates Videos
```

---

## Command Syntax

### Basic Structure

```
command [options] [arguments]
   │       │           │
   │       │           └── What to operate on (file, dir, user)
   │       └── How to operate (modifiers)
   └── What to do (action)
```

### Understanding Options

Options change a command's behavior. They usually start with a hyphen (`-`).

**Single-letter options:**
```bash
ls -l        # Long listing
ls -a        # Show hidden files (starting with .)
ls -t        # Sort by time

# Grouping options (equivalent)
ls -l -a -t
ls -lat      # More common way to type it
```

**Full-word options:**
```bash
# Usually start with double hyphen (--)
hostname --help
ls --all              # Same as -a
```

**Note:** Most commands support `--help` to see available options and arguments.

---

## Arguments

An argument is an extra piece of information (filename, directory, username, device) that tells the command **what** to act on.

### Basic Arguments
```bash
# "Display contents of /etc/passwd"
cat /etc/passwd
#    └── Argument (file to read)
```

### Options with Arguments
Some options require their own arguments.

**Single-letter options with arguments:**
```bash
# Create archive verbosely from /home/chris
tar -cvf backup.tar /home/chris
#        │          └── Argument for command (source dir)
#        └── Argument for 'f' option (filename)
```
In this `tar` example:
- `c` = create
- `v` = verbose (show progress)
- `f` = file (requires a filename argument immediately after)

**Full-word options with arguments:**
```bash
# Connect with equals sign (=) or space
ls --hide=Desktop
#         └── Argument for --hide option
```
This tells `ls` *not* to display "Desktop" in the list.

---

## Essential Commands from the Book

### 1. The `ls` Command (Listing Files)

```bash
# Basic list
ls

# Show hidden files (start with .)
ls -a
# Output includes: .  ..  .bashrc  .profile

# Long listing (permissions, owner, size, date)
ls -l

# Combined
ls -la
```

### 2. The `uname` Command (System Info)

```bash
# System name only
uname
# Output: Linux

# All information (-a)
uname -a
# Output: Linux mydesktop 5.3.7-301.fc31.x86_64 ... GNU/Linux
# (Shows hostname, kernel version, release date, architecture)
```

### 3. The `date` Command (Time & Date)

```bash
# Default output
date

# Formatting output (starts with +)
date +'%d/%m/%y'
# Output: 04/03/20

date +'%A, %B %d, %Y'
# Output: Wednesday, March 04, 2020
```
> **Tip:** Type `date --help` to see all formatting codes (like `%A` for full weekday name).

### 4. The `id` Command (User Identity)

Used to find out who you are and what groups you belong to.

```bash
id
# Output:
# uid=1000(chris) gid=1000(chris) groups=1005(sales), 7(lp)
```
- **uid**: User ID number
- **gid**: Primary Group ID number
- **groups**: All groups the user belongs to
- **context**: SELinux security context (if enabled)

### 5. The `who` Command (User Identification)

Shows who is currently logged into the system.

```bash
# -u (add idle time/PID), -H (print header)
who -uH

# Output:
# NAME    LINE    TIME           IDLE   PID   COMMENT
# chris   tty1    Jan 13 20:57   .      2019
```
- **LINE**: Terminal device (tty1 = local console)
- **IDLE**: How long since last specific action (`.` means active now)
- **PID**: Process ID of the login shell

---

## Locating Commands

### Where Commands Live

```
Standard directories in PATH:
├── /bin          → Essential user binaries (ls, cp, mv)
├── /sbin         → System/admin binaries (fdisk)
├── /usr/bin      → User programs
└── /usr/sbin     → Admin programs
```

### Finding Command Locations

```bash
# 1. which - Find executable in PATH
which pwd
# Output: /usr/bin/pwd

# 2. whereis - Find binary, source, and man page
whereis ls
# Output: ls: /bin/ls /usr/share/man/man1/ls.1.gz

# 3. type - Show how shell interprets it
type ls
# Output: ls is aliased to 'ls --color=auto'
type cd
# Output: cd is a shell builtin
```

---

## Review Questions

1. What command displays the current working directory?

2. How do you group single-letter options (like `-l`, `-a`, `-t`)?

3. What does the `uname -a` command show?

4. In the command `tar -cvf backup.tar`, what is `backup.tar`?

5. What symbol is used to format the output of the `date` command?

6. What command shows your user ID (uid) and group IDs?

7. In `ls --hide=Desktop`, what is `--hide` and what is `Desktop`?

8. What does the `who` command display?

---

## Quick Reference

```
Basic Commands:
├── date              → Show date/time
├── pwd               → Print working directory
├── hostname          → Show computer name
├── ls                → List files
├── uname -a          → System/kernel info
├── id                → User identity info
└── who -uH           → Logged-in users info

Syntax:
├── -a                → Short option
├── --all             → Long option
├── -lat              → Grouped options
└── --hide=Desktop    → Option with argument

Formatting Date:
└── date +'%A, %B %d' → Custom format
```
