# Sample Scripts: Phone List & Backup

## Script 1: Simple Phone List (`ph`)

This script manages a simple text-based "database" of names and numbers.

**Logic:**
1.  Check if arguments exist (`$#`).
2.  If arg1 is "new", append remaining args ($*) to file.
3.  Else, assume arg1 is a search term and `grep` for it.

```bash
#!/bin/bash
PHONELIST=~/.phonelist.txt

# 1. Check for arguments
if [ $# -lt 1 ]; then
    echo "Usage: ph [new name number] | [name]"
    exit 1
fi

# 2. Add Mode
if [ "$1" = "new" ]; then
    shift               # Remove "new" from $1, shifting args left
    echo "$*" >> $PHONELIST
    echo "Added: $*"
    exit 0
fi

# 3. Search Mode
if [ ! -s $PHONELIST ]; then
    echo "Phone list is empty!"
    exit 1
else
    grep -i "$*" $PHONELIST
fi
exit 0
```

---

## Script 2: Home Directory Backup

This script finds home directories and archives them using `tar`.

**Logic:**
1.  Define Tape/Destination.
2.  Get list of home directories from `/etc/passwd` using `cut`.
3.  Run `tar` to archive them.

```bash
#!/bin/bash
DEST="/tmp/backup.tar"  # Changed from Tape for modern context

# 1. Get List of Homes
# grep for /home, cut field 6 (path)
HOMES=$(grep /home /etc/passwd | cut -f6 -d':')

echo "Backing up the following directories:"
echo "$HOMES"

# 2. Archive
# c=create, v=verbose, f=file
tar cvf $DEST $HOMES

echo "Backup Complete to $DEST"
```

---

## Key Takeaways

1.  **Automation:** Both scripts automate multi-step processes (checking file, grepping, formatting).
2.  **Input Handling:** Both rely on `$1` or `$*` to react to user input.
3.  **Pipelines:** The backup script relies heavily on `grep` | `cut` to generate dynamic data.
4.  **Exit Codes:** Good scripts use `exit 0` for success and `exit 1` for errors.

---

## Review Questions

1. In the phone script, what does the `shift` command do?

2. What does `>>` do in the phone script?

3. Why does the backup script use `$(...)` syntax?

4. Which command is used to actually create the backup archive?

5. What does `grep -i` do in the phone script?

6. If `$#` is less than 1, what does the phone script do?

7. True or False: You can chain `grep` and `cut` together with a pipe.

8. What is the standard exit code for "Success"?

---

## Quick Reference

```
Redirects:
>> file      → Append to file
> file       → Overwrite file

Commands:
shift        → Move args ($2 becomes $1)
exit n       → Quit script with status n
tar cvf      → Create a tar archive
```
