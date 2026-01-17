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

### 📂 Section 2: Moving Around the Filesystem

| # | Topic | Notes File |
|---|-------|------------|
| 8 | Filesystem Hierarchy | [08-filesystem-hierarchy.md](../Notes/part-2/08-filesystem-hierarchy.md) |
| 9 | Navigating & Managing | [09-navigating-managing-files.md](../Notes/part-2/09-navigating-managing-files.md) |
| 10 | Permissions & Ownership | [10-permissions-ownership.md](../Notes/part-2/10-permissions-ownership.md) |
| 11 | Moving, Copying, Deleting | [11-moving-copying-deleting.md](../Notes/part-2/11-moving-copying-deleting.md) |

### 📝 Section 3: Working with Text Files

| # | Topic | Notes File |
|---|-------|------------|
| 12 | Editing with Vim | [12-editing-with-vim.md](../Notes/part-2/12-editing-with-vim.md) |
| 13 | Finding Files | [13-finding-files.md](../Notes/part-2/13-finding-files.md) |
| 14 | Searching Text | [14-searching-text-grep.md](../Notes/part-2/14-searching-text-grep.md) |

### ⚙️ Section 4: Managing Processes

| # | Topic | Notes File |
|---|-------|------------|
| 15 | Viewing Processes | [15-viewing-processes.md](../Notes/part-2/15-viewing-processes.md) |
| 16 | Job Control | [16-background-foreground-jobs.md](../Notes/part-2/16-background-foreground-jobs.md) |
| 17 | Killing & Renicing | [17-killing-renicing-processes.md](../Notes/part-2/17-killing-renicing-processes.md) |

### 📜 Section 5: Shell Scripting

| # | Topic | Notes File |
|---|-------|------------|
| 18 | Script Basics | [18-shell-script-basics.md](../Notes/part-2/18-shell-script-basics.md) |
| 19 | Control Flow | [19-control-flow-arithmetic.md](../Notes/part-2/19-control-flow-arithmetic.md) |
| 20 | Loops & Filters | [20-loops-text-filters.md](../Notes/part-2/20-loops-text-filters.md) |
| 21 | Sample Scripts | [21-sample-scripts.md](../Notes/part-2/21-sample-scripts.md) |

---

## 🎯 Learning Objectives

By the end of this part, you should be able to:
- **Understand Shells:** Distinguish between shells, terminals, and consoles.
- **Run Commands:** Execute commands with options, arguments, and proper syntax.
- **Manage History:** Recall and edit previous commands efficiently.
- **Connect Commands:** Use pipes and redirection to build complex workflows.
- **Navigate Filesystem:** Understand the Linux directory tree and move around (`cd`, `pwd`).
- **Manage Files:** Create, copy, move, and delete files and directories.
- **Control Permissions:** Understand and modify file permissions (`chmod`, `chown`).
- **Edit Text:** Use `vim` to create and modify files mainly.
- **Find Content:** Locate files (`find`) and search text content (`grep`).
- **Manage Processes:** View (`ps`, `top`), kill (`kill`), and manage background jobs.
- **Write Scripts:** Create basic shell scripts with variables, loops, and logic.
- **Use Documentation:** Find help using `man`, `info`, and `--help`.

## ⌨️ Key Commands

```bash
# Shell & History
history, !, !!, fc, alias, export, echo

# Getting Info
man, info, help, type, which, whatis

# Filesystem Navigation
cd, pwd, ls, mkdir, rmdir

# File Management
cp, mv, rm, touch, file

# Permissions
chmod, chown, umask, id, groups

# Processes
ps, top, jobs, kill, killall, nice, renice, fg, bg

# Text & Search
vi, vim, find, locate, grep, updatedb

# Scripting
bash, sh, chmod, read, test, expr, let, cut, tr, sed, awk

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