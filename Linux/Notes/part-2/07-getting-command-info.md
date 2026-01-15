# Getting Information About Commands

## Key Concepts

| Tool | Purpose |
|------|---------|
| **man** | Detailed manual pages |
| **help** | Help for shell built-in commands |
| **info** | Hierarchical info documentation |
| **--help** | Quick command usage summary |

---

## Sources of Info

The book suggests checking in this order:

1.  **Check the PATH:**
    `echo $PATH` shows where commands live.
    `ls /bin` lists standard commands.

2.  **Use `help` (for built-ins):**
    Built-in commands (like `cd`, `pwd`, `alias`) don't have files.
    ```bash
    help cd
    help | less    # List all built-ins
    ```

3.  **Use `--help`:**
    Most commands support this option.
    ```bash
    date --help
    fdisk -h
    ```

4.  **Use `info`:**
    GNU project's hypertext documentation system.
    ```bash
    info date
    ```

5.  **Use `man`:**
    The classic manual pages.
    ```bash
    man passwd
    ```

---

## Man Page Sections

Manual pages are organized into numbered sections.

| Section | Content | Target Audience |
|---------|---------|-----------------|
| **1** | **User Commands** | Regular users |
| **2** | System Calls | Programmers |
| **3** | C Library Functions | Programmers |
| **4** | Devices/Special Files | Admins/Programmers |
| **5** | **File Formats** | Admins (`/etc/passwd` format) |
| **6** | Games | Gamers |
| **7** | Miscellaneous | General info |
| **8** | **System Admin Tools** | Root/Admins |

### Specifying Sections

Sometimes a name exists in multiple sections (e.g., `passwd`).

```bash
man passwd        # Shows Section 1 (Command)
man 5 passwd      # Shows Section 5 (File format)
```

### Searching Man Pages

Search names and descriptions with `-k` (keyword).

```bash
man -k passwd
# Output:
# passwd (1) - update user's authentication tokens
# passwd (5) - password file
```

> **Note:** If `man -k` returns nothing, run `mandb` as root to index the database.

---

## Navigating Man Pages

| Key | Action |
|-----|--------|
| `Page Down` | Next page |
| `Page Up` | Previous page |
| `/term` | Search forward for "term" |
| `n` | Next match |
| `N` | Previous match |
| `q` | Quit |

---

## Review Questions

1. Which section of the man pages contains User Commands?

2. How do you ask for the man page of the `passwd` **file** (not command)?

3. What command would you use to get help on `cd`?

4. What option (`-?`) allows you to search man page descriptions?

5. What does `fdisk -h` typically do?

6. How do you quit a man page?

7. Which command lists all built-in shell commands?

8. If `man -k` doesn't work, what root command might fix it?

---

## Quick Reference

```
Help Tools:
├── command --help    → Quick usage
├── help command      → For built-ins (cd, alias)
├── man command       → Manual page
└── info command      → Info docs

Man Pages:
├── man 1 cmd         → User command
├── man 5 file        → File format
├── man 8 admin       → Admin command
└── man -k keyword    → Search keywords
```
