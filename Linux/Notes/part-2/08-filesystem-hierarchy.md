# Linux Filesystem Hierarchy

## Key Concepts

| Term | Definition |
|------|------------|
| **Filesystem** | Structure where all information (files, dirs) is stored |
| **Root (`/`)** | The top of the command hierarchy |
| **Path** | Location of a file (e.g., `/home/joe/file.txt`) |
| **Absolute Path** | Path starting from root `/` |
| **Relative Path** | Path starting from current directory |

---

## The Linux Directory Tree

Linux uses a single hierarchical tree structure, unlike Windows which uses drive letters (`C:\`, `D:\`).

```
/ (Root)
├── bin          → Common user commands (ls, sort, date)
├── boot         → Kernel and boot loader files
├── dev          → Device files (tty*, hd*, sd*)
├── etc          → Configuration files (passwd, group)
├── home         → User home directories (/home/joe)
├── lib          → Shared libraries
├── media        → Mount point for removable media (USB)
├── mnt          → Temporary mount point
├── opt          → Add-on application software
├── proc         → System resources info
├── root         → Root user's home directory
├── sbin         → Admin commands (fdisk)
├── tmp          → Temporary files
├── usr          → User programs, docs, libraries
└── var          → Variable data (logs, mail, websites)
```

> **Note:** `/usr` contains files that don't change often, while `/var` contains files that change frequently.

---

## Linux vs. Windows Filesystems

The book highlights these key differences:

1.  **Drive Letters:** Windows uses `C:\`, `D:\`. Linux attaches all drives to the single `/` tree.
2.  **Separators:** Windows uses backslash `\`. Linux uses forward slash `/`.
3.  **Extensions:** Windows relies on extensions (`.exe`, `.txt`). Linux generally doesn't require them; permissions determine executability.
4.  **Case Sensitivity:** Linux is case-sensitive (`File.txt` != `file.txt`). Windows is generally not.

---

## Identifying Directories

Special shortcuts for navigation:

| Symbol | Meaning |
|--------|---------|
| `/` | Root directory |
| `~` | Your home directory (`$HOME`) |
| `~user` | Another user's home directory |
| `.` | Current directory (`$PWD`) |
| `..` | Parent directory (one level up) |
| `-` | Previous working directory (`$OLDPWD`) |

---

## Review Questions

1. What is the difference between `/root` and `/`?

2. Which directory contains configuration files?

3. Which directory is used for temporary removable media like USB drives?

4. How would you refer to the parent directory of your current location?

5. True or False: Linux uses drive letters like `C:` for hard drives.

6. What symbol represents the user's home directory?

7. Where are log files typically stored?

8. What is the difference between an absolute path and a relative path?

---

## Quick Reference

```
Standard Directories:
├── /bin, /usr/bin    → User commands
├── /sbin, /usr/sbin  → Admin commands
├── /etc              → Configuration
├── /home             → Users
└── /var/log          → Logs

Shortcuts:
├── /                 → Root
├── ~                 → Home
├── .                 → Here
└── ..                → Up one level
```
