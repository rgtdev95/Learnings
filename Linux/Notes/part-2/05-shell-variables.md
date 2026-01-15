# Using Shell Variables

## Key Concepts

| Term | Definition |
|------|------------|
| **Variable** | Named storage for information used by the shell |
| **Environment Variable** | Variable exported to new shells/subprocesses |
| **Local Variable** | Variable visible only in the current shell |

---

## Understanding Variables

Top check all variables set in your current shell:
```bash
set     # Lists all variables
env     # Lists environment variables only
```

To see a specific variable:
```bash
echo $USER
# chris
```

---

## Common Shell Environment Variables

The book lists these important variables:

| Variable | Description |
|----------|-------------|
| **BASH** | Path to bash command (usually `/bin/bash`) |
| **BASH_VERSION** | Current bash version number |
| **HOME** | Your home directory |
| **PATH** | List of directories to search for commands |
| **PS1** | Your primary shell prompt settings |
| **PWD** | Current working directory |
| **OLDPWD** | Previous working directory |
| **RANDOM** | Generates a random number (0-99999) |
| **SECONDS** | Seconds since shell started |
| **HISTFILE** | Location of history file (`~/.bash_history`) |
| **HISTSIZE** | Number of history entries to keep (default 1000) |
| **USER** | Current username |
| **UID** | User ID number |
| **EUID** | Effective User ID number |
| **TMOUT** | Seconds of inactivity before shell exits (security feature) |

---

## Creating Your Own Variables

You can create shortcuts for your work.

**Example from book:**
```bash
M=/work/time/files/info/memos ; export M

# Use it
cd $M
vi $M/hotdog
```

---

## Modifying PATH

To add a directory to your PATH permanently (e.g., `/getstuff/bin`):

```bash
# In your .bashrc file:
PATH=$PATH:/getstuff/bin ; export PATH
```

> **Warning:** Avoid adding `.` (current directory) to your PATH for security reasons. It prevents malicious scripts in the current folder from running when you type a standard command name.

---

## Setting TMOUT (Auto-Logout)

To log out inactive users automatically (e.g., after 30 minutes):

```bash
TMOUT=1800
```
Add this to configuration files for system-wide effect.

---

## Review Questions

1. Which command shows all variables (local and environment)?

2. What variable stores your command history location?

3. What does the `RANDOM` variable do?

4. Why might you set the `TMOUT` variable?

5. How do you print the value of the `USER` variable?

6. What symbol separates directories in the `PATH` variable?

7. Why is adding `.` to your PATH discouraged?

8. What variable holds the previous working directory?

---

## Quick Reference

```
Variables:
├── set               → List all variables
├── env               → List environment variables
├── echo $VAR         → View variable value
└── export VAR=val    → Set environment variable

Key Variables:
├── $HOME             → Home directory
├── $PATH             → Command search path
├── $PS1              → Prompt string
├── $PWD              → Current directory
└── $RANDOM           → Random number
```
