# Navigating and Managing Files

## Key Concepts

| Term | Definition |
|------|------------|
| **cd** | Change Directory |
| **pwd** | Print Working Directory |
| **mkdir** | Make Directory |
| **ls** | List directory contents |
| **Metacharacter** | Special char used for matching (`*`, `?`) |

---

## Basic Navigation Commands

### `pwd` - Where am I?
Prints the absolute path of your current directory.
```bash
$ pwd
/home/joe
```

### `cd` - Change Directory
Moves you around the filesystem.

```bash
cd /usr/share    # Go to absolute path
cd doc           # Go to relative path (inside current)
cd ..            # Go up one level
cd               # Go to your home directory
cd ~             # Go to your home directory
cd -             # Go to previous directory
```

### `mkdir` - Create Directories

```bash
mkdir test              # Create 'test' in current dir
mkdir -p dir1/dir2/dir3 # Create parent dirs if needed
```

---

## Listing Files (`ls`)

The `ls` command shows you what is in a directory.

| Option | Meaning |
|--------|---------|
| `-a` | Show **all** files (including hidden `.` files) |
| `-l` | **Long** listing (permissions, owner, size, date) |
| `-t` | Sort by **time** (newest first) |
| `-r` | **Reverse** sort order |
| `-R` | **Recursive** (list subdirectories too) |
| `-F` | Add type indicator (`/` for dir, `*` for exe) |
| `-S` | Sort by **size** |
| `--hide=pattern` | Hide files matching pattern |

**Example:**
```bash
ls -lat
# Lists all files, detailed info, sorted by newest first
```

---

## File Matching Metacharacters

Use wildcards to match multiple files at once.

| Char | Matches | Example |
|------|---------|---------|
| `*` | Any number of chars | `ls *.txt` (all txt files) |
| `?` | Any ONE char | `ls file?.txt` (file1.txt, fileA.txt) |
| `[...]` | Any char in set | `ls [abc]*` (starts with a, b, or c) |
| `[a-z]` | Any char in range | `ls [a-m]*` (starts with a through m) |

**Brace Expansion `{}`**
Create patterns or sequences.
```bash
touch memo{1,2,3}        # Creates memo1, memo2, memo3
touch {a..c}{1..3}       # Creates a1, a2, a3, b1... c3
```

---

## Review Questions

1. Which command creates a directory and its parents (e.g., `mkdir -p a/b/c`)?

2. What does `cd ..` do?

3. How do you list hidden files?

4. What does the `*` wildcard match?

5. If you are in `/usr/bin`, what command takes you to `/usr`?

6. How does `ls -F` mark directories?

7. What command sorts files by size?

8. Create a command to list all files ending in `.conf`.

---

## Quick Reference

```
Navigation:
├── cd /path     → Go to absolute
├── cd dir       → Go to relative
├── cd ..        → Go up
└── pwd          → Show location

Listing:
├── ls -a        → Show hidden
├── ls -l        → Show details
└── ls -R        → Recursive

Wildcards:
├── *            → Any length
└── ?            → Single char
```
