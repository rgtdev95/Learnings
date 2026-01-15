# Part 2: Becoming a Linux Power User

> Based on Linux Bible, 10th Edition by Christopher Negus

## Topics Overview

This section covers essential command-line skills to become proficient with the Linux shell.

---

## Study Materials

### 📚 Detailed Notes

| # | Topic | Notes File |
|---|-------|------------|
| 1 | Shells, Terminals, and Consoles | [01-shells-terminals-consoles.md](../Notes/part-2/01-shells-terminals-consoles.md) |
| 2 | Running Commands | [02-running-commands.md](../Notes/part-2/02-running-commands.md) |
| 3 | Command History & Editing | [03-command-history-editing.md](../Notes/part-2/03-command-history-editing.md) |
| 4 | Connecting & Expanding Commands | [04-connecting-expanding-commands.md](../Notes/part-2/04-connecting-expanding-commands.md) |
| 5 | Shell Variables | [05-shell-variables.md](../Notes/part-2/05-shell-variables.md) |
| 6 | Shell Environment | [06-shell-environment.md](../Notes/part-2/06-shell-environment.md) |
| 7 | Getting Command Information | [07-getting-command-info.md](../Notes/part-2/07-getting-command-info.md) |

### ❓ Quiz & Flashcards

- [Part 2 Quiz](../Quiz/Part-2/part-2-quiz.md) - 36 questions with collapsible answers

---

## Learning Objectives

After completing Part 2, you should be able to:

- [ ] Explain the difference between shells, terminals, and virtual consoles
- [ ] Understand command syntax and locate commands
- [ ] Use command history and line editing efficiently
- [ ] Connect commands with pipes and redirections
- [ ] Work with shell and environment variables
- [ ] Customize your shell environment
- [ ] Find help and documentation for any command

---

## Key Commands Covered

```bash
# Shell Information
echo $SHELL              # Current shell
cat /etc/shells          # Available shells
chsh -s /bin/zsh         # Change default shell

# Command Location
which command            # Find in PATH
type command             # Show interpretation
whereis command          # Find binary, source, man

# History & Editing
history                  # View history
!!                       # Repeat last command
!$                       # Last argument
Ctrl+R                   # Reverse search

# Pipes & Redirection
cmd1 | cmd2              # Pipe
cmd > file               # Redirect output
cmd 2>&1                 # Redirect stderr to stdout
cmd &                    # Background

# Variables
export VAR=value         # Environment variable
echo ${#var}             # String length
${var:-default}          # Default value

# Help
man command              # Manual page
help builtin             # Built-in help
apropos keyword          # Search man pages
```

---

## Key Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Beginning of line |
| `Ctrl+E` | End of line |
| `Ctrl+U` | Delete to start |
| `Ctrl+K` | Delete to end |
| `Ctrl+R` | Reverse search |
| `Ctrl+C` | Interrupt |
| `Ctrl+Z` | Suspend |
| `Tab` | Auto-complete |

---

## Quick Summary

| Topic | Key Takeaway |
|-------|--------------|
| Shells | bash, zsh, fish - command interpreters |
| Commands | `command [options] [arguments]` |
| History | `!!`, `!$`, `Ctrl+R` for efficiency |
| Pipes | `cmd1 \| cmd2` connects output → input |
| Variables | `$VAR` local, `export VAR` environment |
| Config | `~/.bashrc` for customization |
| Help | `man`, `help`, `apropos` |

---

## Next Steps

After mastering Part 2, proceed to:
- **Part 3:** [Linux System Administrator](./part-3-linux-system-administrator.md) - System admin, users, software