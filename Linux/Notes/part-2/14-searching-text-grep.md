# Searching Text with grep

## Key Concepts

| Term | Definition |
|------|------------|
| **grep** | Global Regular Expression Print |
| **Search Term** | The text you are looking for |
| **Pattern** | Advanced matching rules (Regular Expressions) |
| **Recursive** | Searching through folders and subfolders |

---

## Basic Usage

`grep` searches **inside** the content of files (unlike `find`, which looks at file names/metadata).

```bash
grep "search_term" filename
```

**Example:**
```bash
grep "root" /etc/passwd
# Prints any line in /etc/passwd containing "root"
```

---

## Common Options

| Option | Effect |
|--------|--------|
| `-i` | **Ignore case** (match `a` and `A`) |
| `-v` | **Invert** match (show lines that DO NOT match) |
| `-n` | Show **line numbers** |
| `-r` | **Recursive** (search all files in directory) |
| `-l` | Return only **filenames** (not the text content) |
| `--color` | Highlight matches in color |

---

## Examples

### 1. Case Insensitive Search
Find "error", "Error", "ERROR", etc.
```bash
grep -i "error" /var/log/syslog
```

### 2. Recursive Search
Find every file in `/etc` that contains the IP "192.168.1.1".
```bash
grep -r "192.168.1.1" /etc/
```

### 3. Invert Search
Show all lines in a file EXCEPT comments (lines starting with `#`).
```bash
grep -v "^#" /etc/services
```
*(Note: `^` is a regex meaning "start of line")*

### 4. Piping to grep
Filter the output of other commands.
```bash
# Only show lines with "inet" (IP addresses)
ip addr show | grep "inet"

# Check if a process is running
ps aux | grep firefox
```

---

## Review Questions

1. What does `grep -i` do?

2. How do you search for the word "hello" in ALL files in the current folder?

3. Which option shows line numbers?

4. How do you print lines that DO NOT contain a specific word?

5. What is the difference between `grep` and `find`?

6. Write a command to find "success" in `logfile.txt` regardless of case.

7. How can you list just the **names** of files containing a match?

8. If you run `ls -l | grep "^d"`, what are you filtering for?

---

## Quick Reference

```
Basic:
└── grep "text" file     → Search in file

Options:
├── -i                   → Case insensitive
├── -r                   → Recursive (folders)
├── -v                   → Invert (no match)
├── -n                   → Line numbers
└── -l                   → Show filename only

Pipelines:
└── cmd | grep "text"    → Filter output
```
