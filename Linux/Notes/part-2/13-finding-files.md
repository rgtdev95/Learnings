# Finding Files

## Key Concepts

| Tool | Approach | Speed | Freshness |
|------|----------|-------|-----------|
| **`locate`** | Searches a **database** | Instant | 1 day old (usually) |
| **`find`** | Searches **live filesystem** | Slower | Real-time |

---

## Using `locate`

The `locate` command searches a pre-built index of files (`mlocate.db`). This index is updated automatically (usually nightly by a cron job running `updatedb`).

```bash
locate password        # Lists all paths containing "password"
locate -i .bashrc      # Case-insensitive search
```

> **Pros:** Very fast.
>
> **Cons:** Can't find files created just now (until database updates). Privacy (non-root users can only see files they have access to).

---

## Using `find`

The `find` command is powerful and searches the actual directory tree. It can search by name, size, user, time, and more.

**Syntax:** `find [start_path] [options] [action]`

### 1. By Name
```bash
find /etc -name passwd       # Exact match "passwd"
find /home -iname "*jo*"     # Case-insensitive, wildcard match
```

### 2. By Size (`-size`)
Units: `k` (KB), `M` (MB), `G` (GB)
```bash
find / -size +10M            # Files larger than 10MB
find /tmp -size -1k          # Files smaller than 1KB
```

### 3. By User/Group (`-user`, `-group`)
```bash
find /home -user joe         # Owned by joe
find /var -not -user root    # NOT owned by root
```

### 4. By Time
*   `-mtime`: Modified days ago
*   `-mmin`: Modified minutes ago

```bash
find /var/log -mtime -3      # Modified less than 3 days ago
find . -mmin -10             # Modified in the last 10 minutes
```

---

## Acting on Found Files (`-exec`)

You can run commands on every file found using `-exec`.
*   `{}` represents the found filename.
*   `\;` ends the command.

```bash
# Delete all .tmp files in user's home (BE CAREFUL)
find ~ -name "*.tmp" -exec rm {} \;

# Copy all large files to /backup
find . -size +100M -exec cp {} /backup/ \;
```

> **Safety Tip:** Use `-ok` instead of `-exec` to prompt for confirmation for each file.
> `find . -name "*.tmp" -ok rm {} \;`

---

## Review Questions

1. Which command is faster: `find` or `locate`?

2. What command updates the `locate` database?

3. Create a `find` command to find all files ending in `.jpg` in `/home`.

4. How do you find files larger than 1 Gigabyte?

5. What does the `{}` symbol represent in an `-exec` command?

6. How do you find files modified in the last 5 minutes?

7. What option allows you to search for names regardless of case?

8. Why might `locate` fail to find a file you just created?

---

## Quick Reference

```
locate:
└── locate name          → Search DB

find:
├── -name "pattern"      → By name
├── -iname "pattern"     → Case-insensitive
├── -size +10M           → > 10MB
├── -mtime -7            → Modified < 7 days ago
├── -user joe            → Owned by joe
└── -exec cmd {} \;      → Run command on results
```
