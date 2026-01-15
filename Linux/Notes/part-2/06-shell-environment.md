# Creating Your Shell Environment

## Key Concepts

| Term | Definition |
|------|------------|
| **Alias** | A shortcut name for a command and its options |
| **.bashrc** | User-specific configuration file (interactive shells) |
| **.bash_profile** | User login configuration file |
| **Prompt (PS1)** | The text displayed when shell waits for input |

---

## Configuration Files

The book describes these key files for configuring bash:

| File | Scope | Runs When... |
|------|-------|--------------|
| `/etc/profile` | System-wide | Every user logs in |
| `/etc/bashrc` | System-wide | Every bash shell opens |
| `~/.bash_profile` | User-specific | **Login** only (inherits to others) |
| `~/.bashrc` | User-specific | **Every** bash shell open |
| `~/.bash_logout` | User-specific | User logs out |

> **Editing Tip:** Use `nano ~/.bashrc` to edit your configuration.
> To apply changes immediately: `source ~/.bashrc`

---

## Setting Your Prompt (PS1)

Your prompt is defined by the `PS1` variable. You can customize it with special characters:

| Code | Effect |
|------|--------|
| `\u` | Current username |
| `\h` | Hostname |
| `\w` | Full working directory path |
| `\W` | Basename of working directory |
| `\t` | Time (HH:MM:SS) |
| `\d` | Date (Day Month Date) |
| `\$` | Prompt symbol ($ for user, # for root) |
| `\!` | History number of command |

**Example from book:**
```bash
# Temporary change
export PS1="[\t \w]\$ "
# Output: [20:26:32 /var/spool]$
```

---

## Creating Aliases

Aliases are shortcuts for long commands.

**Book Examples:**
```bash
# Combine commands
alias p='pwd ; ls -CF'

# Safety prompts
alias rm='rm -i'
```
The second example prevents accidental deletions by forcing `rm` to ask for confirmation.

**Managing Aliases:**
```bash
alias           # List all aliases
unalias name    # Remove an alias
```

---

## Practical: Creating a Custom Environment

1.  **Edit your .bashrc:**
    ```bash
    nano ~/.bashrc
    ```

2.  **Add an alias:**
    ```bash
    alias d='date +%D'
    ```

3.  **Add a variable:**
    ```bash
    m=/work/project/files ; export m
    ```

4.  **Save and Apply:**
    - Press `Ctrl+O` to save, `Ctrl+X` to exit
    - Run `source ~/.bashrc`

5.  **Test:**
    ```bash
    d
    # 06/29/19
    ```

---

## Review Questions

1. Which configuration file runs every time you open a new bash shell?

2. What command applies changes from `.bashrc` immediately?

3. What does `\u` and `\h` stand for in a prompt?

4. How do you create an alias called `myfiles` that runs `ls -la`?

5. What file allows system-wide configuration for all users?

6. What is the `rm -i` alias useful for?

7. What variable controls your primary prompt?

8. How do you remove an alias?

---

## Quick Reference

```
Config Files:
├── /etc/profile      → System login
├── ~/.bash_profile   → User login
└── ~/.bashrc         → User interactive

Prompt Codes:
├── \u \h \w          → User, Host, Path
└── \t \d             → Time, Date

Aliases:
├── alias name='cmd'  → Set alias
└── unalias name      → Unset alias
```
