# Editing Files with Vim

## Key Concepts

| Term | Definition |
|------|------------|
| **vi** | Visual Editor (standard on all Linux systems) |
| **vim** | Vi IMproved (syntax highlighting, more features) |
| **Command Mode** | For navigation, saving, deleting (Default mode) |
| **Insert Mode** | For typing text |
| **Ex Mode** | For colon `:` commands (search/replace, saving) |

---

## Modes

Vim has two main modes you switch between:

1.  **Command Mode:** (Start here). You cannot type text here. Keys perform actions (delete, move, save).
    *   Press `Esc` to get back to Command Mode.
2.  **Insert Mode:** Type text normally.
    *   Press `i`, `a`, or `o` to enter Insert Mode.

> **Tip:** If you get stuck or don't know what mode you are in, press `Esc` a few times to ensure you are in Command Mode.

---

## Basic Workflow

1.  **Open file:** `vi filename`
2.  **Enter Insert Mode:** Press `i`
3.  **Type Text:** Type your content...
4.  **Exit Insert Mode:** Press `Esc`
5.  **Save and Quit:** Type `:wq` then `Enter`

---

## Navigation (Command Mode)

| Key | Action |
|-----|--------|
| `h` `j` `k` `l` | Left, Down, Up, Right (or use Arrow Keys) |
| `w` | Forward one **word** |
| `b` | Backward one **word** |
| `0` (zero) | Start of line |
| `$` | End of line |
| `1G` or `gg` | Go to **first** line |
| `G` | Go to **last** line |
| `Ctrl+f` | Page **Forward** |
| `Ctrl+b` | Page **Back** |

---

## Editing Commands

**Entering Insert Mode:**
*   `i` = Insert before cursor
*   `a` = Append after cursor
*   `o` = Open new line **below**
*   `O` = Open new line **above**

**Deleting & Changing:**
*   `x` = Delete **character** under cursor
*   `dd` = Delete **line**
*   `dw` = Delete **word**
*   `u` = **Undo** last change
*   `Ctrl+r` = **Redo**
*   `r` = Replace single character

**Copy & Paste:**
*   `yy` = **Yank** (copy) line
*   `p` = **Put** (paste) below cursor

---

## Searching & Saving

| Command | Action |
|---------|--------|
| `/text` | Search **forward** for "text" |
| `?text` | Search **backward** for "text" |
| `n` | Next match |
| `:w` | **Save** (write) |
| `:q` | **Quit** |
| `:q!` | **Quit without saving** (discard changes) |
| `:wq` | **Save and Quit** (or `ZZ`) |

---

## Review Questions

1. Which key puts you into Insert Mode before the cursor?

2. How do you exit Insert Mode?

3. What command deletes the current line?

4. How do you jump to the last line of a file?

5. What does the command `:q!` do?

6. How do you undo a mistake?

7. Which key opens a new line below the current one?

8. True or False: You can type text immediately after opening a file in vi.

---

## Quick Reference

```
Start/End:
├── vi file      → Open
├── i            → Insert text
├── Esc          → Exit insert mode
└── :wq          → Save & Quit

Edits:
├── dd           → Delete line
├── yy           → Copy line
├── p            → Paste
└── u            → Undo
```
