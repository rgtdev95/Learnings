# Moving, Copying, and Removing Files

## Key Concepts

| Command | Action |
|---------|--------|
| **cp** | Copy files or directories |
| **mv** | Move or rename files |
| **rm** | Remove files |
| **rmdir** | Remove **empty** directories |

---

## Copying Files (`cp`)

Copy a source file to a destination.

```bash
cp file1 file2           # Copy file1 to file2
cp file1 ~/Documents     # Copy to directory
```

### Recursive Copy
To copy a directory and its contents, use `-r` (recursive) or `-a` (archive - preserves permissions/times).

```bash
cp -r dir1 dir2          # Copy dir1 content to dir2
cp -a /source /dest      # Best for backups (preserves attributes)
```

> **Safety:** Use `cp -i` to prompt before overwriting.

---

## Moving Files (`mv`)

Used for both **moving** and **renaming** files.

```bash
mv oldname.txt newname.txt   # Rename
mv file.txt ~/Documents/     # Move
```

> **Warning:** `mv` overwrites files silently by default unless configured with `-i`.

---

## Removing Files (`rm`)

Delete files permanently. **There is no Trash/Recycle Bin in the shell.**

```bash
rm file.txt
rm -i file.txt       # Prompt for confirmation (Interactive)
```

### Removing Directories
`rm` cannot remove directories without options.

```bash
rmdir emptybox       # Remove ONLY if empty
rm -r folder         # Recursive remove (folder + contents)
rm -rf folder        # Force recursive remove (NO PROMPT)
```

> **DANGER:** `rm -rf /` or `rm -rf *` can destroy your system. Always check where you are (`pwd`) before running destructive commands.

---

## File Redirection

Send input/output to files instead of the screen.

| Operator | Action | Example |
|----------|--------|---------|
| `>` | Redirect stdout (overwrite) | `ls > listing.txt` |
| `>>` | Redirect stdout (append) | `date >> log.txt` |
| `<` | Redirect stdin | `mail root < message.txt` |
| `2>` | Redirect stderr (errors) | `ls /fake 2> errors.txt` |
| `&>` | Redirect both out & err | `make &> build.log` |

---

## Review Questions

1. Which command renames a file?

2. How do you copy a directory?

3. What flag makes `rm` ask for confirmation?

4. What is the difference between `>` and `>>`?

5. How do you delete a non-empty directory?

6. What does `cp -a` do differently from `cp -r`?

7. What command removes an empty directory safely?

8. How do you redirect error messages to a file?

---

## Quick Reference

```
Manipulation:
├── cp source dest      → Copy
├── mv source dest      → Move/Rename
├── rm file             → Delete
└── rmdir dir           → Delete empty dir

Options:
├── -r, -R              → Recursive
├── -i                  → Interactive (ask me)
├── -f                  → Force (no asking)
└── -a                  → Archive (preserve mode)

Redirection:
├── > file              → Overwrite
└── >> file             → Append
```
