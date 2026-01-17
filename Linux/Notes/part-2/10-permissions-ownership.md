# File Permissions and Ownership

## Key Concepts

| Term | Definition |
|------|------------|
| **Permission Bits** | 9 bits defining access (`rwxrwxrwx`) |
| **Owner** | User who owns the file |
| **Group** | Group of users with specific access |
| **chmod** | Change Mode (permissions) |
| **chown** | Change Owner |
| **umask** | Default permission mask for new files |

---

## Understanding `ls -l` Output

When you run `ls -l` (long listing), you see 10 characters at the start of each line:

```
-rwxr-xr-x 1 joe sales 4960 Jan 18 10:00 myfile
```

| Position | Character | Meaning |
|----------|-----------|---------|
| 1 | `-` | **File Type** (`-` regular, `d` directory, `l` link) |
| 2-4 | `rwx` | **Owner** Permissions (User) |
| 5-7 | `r-x` | **Group** Permissions |
| 8-10 | `r-x` | **Other** Permissions (Everyone else) |

### Permission Meanings

| Permission | On a File | On a Directory |
|------------|-----------|----------------|
| **Read (r)** | View contents (`cat`) | List contents (`ls`) |
| **Write (w)** | Modify/Delete content | Add/Remove files inside |
| **Execute (x)** | Run as program | Enter directory (`cd`) |

---

## Changing Permissions (`chmod`)

You can change permissions using **numbers** (octal) or **letters** (symbolic).

### Numeric Mode (Octal)

| Mode | Binary | Value | Meaning |
|------|--------|-------|---------|
| r | 100 | **4** | Read |
| w | 010 | **2** | Write |
| x | 001 | **1** | Execute |

**Examples:**
```bash
chmod 777 file   # rwx(7) rwx(7) rwx(7) - Full access for everyone
chmod 755 file   # rwx(7) r-x(5) r-x(5) - Owner full, others read/exec
chmod 644 file   # rw-(6) r--(4) r--(4) - Owner read/write, others read
chmod -R 755 dir # Recursive change
```

### Symbolic Mode (Letters)

- **Who:** `u` (user), `g` (group), `o` (other), `a` (all)
- **Action:** `+` (add), `-` (remove), `=` (set)
- **What:** `r`, `w`, `x`

**Examples:**
```bash
chmod a+x script.sh    # Make executable for everyone
chmod go-w file        # Remove write for group and other
chmod u+rw file        # Add read/write for owner
```

---

## Ownership (`chown`)

Only the root user can change file ownership.

```bash
chown joe file.txt       # Change owner to joe
chown joe:sales file.txt # Change owner to joe, group to sales
chown -R joe dir/        # Recursive change
```

---

## Default Permissions (`umask`)

When you create a file, `umask` determines default permissions.
- Default file: `666` (rw-rw-rw-) minus umask
- Default dir: `777` (rwxrwxrwx) minus umask

**Common Umask:** `0022`
- Files become `644` (rw-r--r--)
- Dirs become `755` (rwxr-xr-x)

---

## Review Questions

1. What does the permission `-rwxr-xr-x` mean numerically?

2. How do you give **search/enter** permission to a directory?

3. Which command changes file ownership?

4. If a file has `r--` permission for the owner, can they write to it?

5. What does `chmod u+x` do?

6. How do you recursively change permissions for a directory?

7. What happens if you remove **execute** permission from a directory?

8. What is the value of **read** permission in octal?

---

## Quick Reference

```
Permissions:
├── chmod 755    → Owner=All, Others=Read/Exec
├── chmod +x     → Make executable
└── chmod -R     → Recursive

Ownership:
└── chown user:group file

Values:
├── Read = 4
├── Write = 2
└── Link/Exec = 1
```
