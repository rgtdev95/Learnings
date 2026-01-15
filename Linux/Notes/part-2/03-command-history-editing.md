# Command History and Editing

## Key Concepts

| Term | Definition |
|------|------------|
| **History** | List of previously executed commands |
| **Command Completion** | Tab-based auto-completion of commands and files |
| **Command Line Editing** | Using keyboard shortcuts to edit the current line |
| **Variable Expansion** | Using Tab to complete variable names |

---

## Command History

The bash shell saves your commands in a list. When you exit, this list is saved to `~/.bash_history` (typically stores 1000 commands).

### Viewing and Recalling History

```bash
# View recent history (last 8 commands)
history 8
# Output example:
# 382 date
# 383 ls /usr/bin | sort
# ...

# Recall by number (!n)
!382             # Runs command #382

# Recall previous command (!!)
!!               # Runs the very last command

# Recall by search (!?string?)
!?date?          # Runs most recent command containing "date"
```

> **Safety Tip:** When using `!`, the command runs immediately without confirmation.

### Searching History

| Keystroke | Action |
|-----------|--------|
| `Ctrl+R` | **Reverse incremental search:** Type to find previous commands |
| `Ctrl+S` | **Forward incremental search** |
| `Alt+P` | **Reverse search** (non-incremental) |
| `Alt+N` | **Forward search** (non-incremental) |
| `Up/Down` | Step through history command by command |

### Advanced History Editing (fc)

The `fc` (Fix Command) tool opens commands in your default editor (usually vi or nano).

```bash
# Edit and run the last command
fc

# Edit commands 100 through 105
fc 100 105
```

---

## Command-Line Editing

Bash uses **emacs-style** editing keys by default.

### Navigation Shortcuts

| Keystroke | Full Name | Meaning |
|-----------|-----------|---------|
| `Ctrl+A` | Beginning of line | Go to start of line |
| `Ctrl+E` | End of line | Go to end of line |
| `Ctrl+F` | Char forward | Move cursor right |
| `Ctrl+B` | Char backward | Move cursor left |
| `Alt+F` | Word forward | Move one word right |
| `Alt+B` | Word backward | Move one word left |
| `Ctrl+L` | Clear screen | Clear screen, keep line at top |

### Editing Shortcuts

| Keystroke | Meaning |
|-----------|---------|
| `Ctrl+D` | Delete current character |
| `Backspace` | Delete previous character |
| `Ctrl+T` | Transpose characters (switch current and previous) |
| `Alt+T` | Transpose words |
| `Alt+U` | Uppercase current word |
| `Alt+L` | Lowercase current word |
| `Alt+C` | Capitalize current word |

### Cutting and Pasting

| Keystroke | Meaning |
|-----------|---------|
| `Ctrl+K` | Cut text to end of line |
| `Ctrl+U` | Cut text to beginning of line |
| `Ctrl+W` | Cut previous word |
| `Alt+D` | Cut next word |
| `Ctrl+Y` | Paste (“Yank”) most recently cut text |
| `Alt+Y` | Rotate back to previously cut text and paste |
| `Ctrl+C` | Delete/Cancel whole line |

> **Vi Mode:** Prefer vi keys? Add `set -o vi` to your `~/.bashrc`.

---

## Command-Line Completion

Save keystrokes by using the **Tab** key to complete partially typed text.

### Types of Completion

1.  **Command/Alias/Function:**
    ```bash
    userm<Tab>   →  usermod
    ```

2.  **Variables ($):**
    ```bash
    echo $OS<Tab>  →  echo $OSTYPE
    ```

3.  **Usernames (~):**
    ```bash
    cd ~ro<Tab>    →  cd ~root/
    ```

4.  **Hostnames (@):**
    ```bash
    ssh user@loc<Tab>  →  ssh user@localhost
    ```

> **Double Tab:** Press Tab twice to see all possible completions if there's more than one match.

### Example

```bash
$ echo $P<Tab><Tab>
# Displays: $PATH $PPID $PS1 $PS2 $PS4 $PWD
```

---

## Review Questions

1. Which keystroke moves the cursor to the beginning of the command line?

2. How do you perform a reverse incremental search through history?

3. What does `!!` do?

4. Which command recalls command number 50 from your history?

5. What keystroke cuts the text from the cursor to the end of the line?

6. How do you see all possible completions for a partial command?

7. What does `Alt+U` do to the current word?

8. Which file stores your command history?

---

## Quick Reference

```
Editing Keys:
├── Ctrl+A / Ctrl+E   → Start / End of line
├── Ctrl+U / Ctrl+K   → Cut to Start / End
├── Ctrl+W            → Cut previous word
├── Ctrl+Y            → Paste cut text
└── Ctrl+L            → Clear screen

History:
├── history           → View list
├── !n                → Run command #n
├── !!                → Run previous command
└── Ctrl+R            → Search history

Completion:
├── Tab               → Complete text
└── Tab+Tab           → List possibilities
```
