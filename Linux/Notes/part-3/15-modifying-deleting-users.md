# Modifying and Deleting Users

## Modifying Users with usermod

The `usermod` command changes existing user account settings.

### Syntax

```bash
usermod [options] username
```

### Common Options

| Option | Purpose | Example |
|--------|---------|---------|
| `-c "comment"` | Change full name/description | `-c "Jake Jackson"` |
| `-d home_dir` | Change home directory | `-d /newhome/jake` |
| `-e YYYY-MM-DD` | Set expiration date | `-e 2025-12-31` |
| `-g group` | Change primary group | `-g developers` |
| `-G grouplist` | Set supplementary groups | `-G wheel,sales` |
| `-aG grouplist` | **Add** to supplementary groups | `-aG docker` |
| `-l newname` | Change login name | `-l john` |
| `-L` | Lock account | (adds ! to password) |
| `-U` | Unlock account | (removes !) |
| `-m` | Move home directory contents | (use with `-d`) |
| `-s shell` | Change shell | `-s /bin/zsh` |
| `-u uid` | Change UID | `-u 1500` |

---

## Common usermod Examples

### Change Shell

```bash
sudo usermod -s /bin/csh chris
```

### Add to Supplementary Groups (IMPORTANT!)

```bash
# WRONG - Removes existing groups!
sudo usermod -G sales,marketing chris

# CORRECT - Adds to existing groups
sudo usermod -aG sales,marketing chris
```

**Warning:** Always use `-aG` (append to groups) unless you explicitly want to replace all supplementary groups!

### Change Home Directory and Move Files

```bash
sudo usermod -d /home/newlocation -m chris
```

The `-m` option moves existing files to new directory.

### Lock/Unlock Account

```bash
# Lock (disable login)
sudo usermod -L chris

# Unlock
sudo usermod -U chris
```

**How it works:** Adds `!` to encrypted password in `/etc/shadow`

```
# Locked:
chris:!$6$ABC123...:18000:0:99999:7:::

# Unlocked:
chris:$6$ABC123...:18000:0:99999:7:::
```

### Set Account Expiration

```bash
# Expire on specific date
sudo usermod -e 2025-10-15 chris

# Never expire
sudo usermod -e "" chris
```

### Change Username

```bash
sudo usermod -l john jake
```

Changes login name from `jake` to `john`.

---

## Modifying Users via Cockpit

**Access Cockpit:** `http://hostname:9090`

### Available Modifications

1. **Full Name** - Can change anytime
2. **Roles:**
   *   Server Administrator (adds to `wheel` group)
   *   Image Builder (adds to `weldr` group)
3. **Access:**
   *   Lock account
   *   Set expiration date
4. **Password:**
   *   Set new password
   *   Force change on next login
   *   Set password expiration policy
5. **SSH Keys:**
   *   Add public SSH keys for passwordless login

**Changes take effect immediately** - No save button needed

---

## Deleting Users with userdel

### Basic Deletion

```bash
sudo userdel chris
```

**Removes:**
*   Entry from `/etc/passwd`
*   Entry from `/etc/shadow`
*   Entry from `/etc/group` (if private group)

**Keeps:**
*   Home directory
*   User's files throughout system

### Delete User and Home Directory

```bash
sudo userdel -r chris
```

The `-r` option removes:
*   User's home directory
*   User's mail spool (if exists)

**Still keeps:**
*   Files owned by user elsewhere on system

---

## Finding Orphaned Files

After deleting a user, files may remain owned by the former UID.

### Find Files by Username (Before Deletion)

```bash
sudo find / -user chris -ls 2>/dev/null
```

### Find Files by UID (After Deletion)

```bash
# Find files owned by UID 1002
sudo find / -uid 1002 -ls 2>/dev/null
```

### Find All Files Without a User

```bash
sudo find / -nouser -ls 2>/dev/null
```

**Security Risk:** Orphaned files (no owner) are a security concern. Assign them to a real user or delete them.

---

## Reassigning Orphaned Files

### Change Owner of All Files

```bash
# Change all files owned by old UID to new user
sudo find / -uid 1002 -exec chown newuser {} \;
```

### Change Specific Directory

```bash
sudo chown -R newuser:newgroup /path/to/directory
```

---

## User Account Lifecycle Best Practices

### Before Deleting a User

1. **Find all their files:**
   ```bash
   sudo find / -user username -ls > /tmp/username_files.txt
   ```

2. **Back up important data:**
   ```bash
   sudo tar -czf /backup/username_backup.tar.gz /home/username
   ```

3. **Review processes:**
   ```bash
   ps -u username
   ```

4. **Lock account first (testing period):**
   ```bash
   sudo usermod -L username
   ```

5. **Delete after confirming no issues:**
   ```bash
   sudo userdel -r username
   ```

6. **Handle orphaned files:**
   ```bash
   sudo find / -nouser -ls
   ```

---

## Password Management

### Set Password (as root)

```bash
sudo passwd username
```

**As root, you can:**
*   Set short passwords
*   Set blank passwords
*   Bypass complexity requirements

### Force Password Change

```bash
sudo passwd -e username
```

User must change password on next login.

### Lock/Unlock with passwd

```bash
# Lock
sudo passwd -l username

# Unlock
sudo passwd -u username

# Check status
sudo passwd -S username
```

---

## Account Expiration

### Set Expiration Date

```bash
# Expire October 15, 2025
sudo usermod -e 2025-10-15 username

# Check expiration
sudo chage -l username
```

### Password Aging

```bash
# Configure password policies
sudo chage username
```

**Options:**
*   Minimum days between changes
*   Maximum days before must change
*   Warning days before expiration
*   Inactive days after expiration

---

## Batch User Operations

### Modify Multiple Users

```bash
# Add multiple users to a group
for user in alice bob charlie; do
    sudo usermod -aG developers $user
done
```

### Create Multiple Users from File

```bash
# users.txt format: username:fullname:group
while IFS=: read user fullname group; do
    sudo useradd -c "$fullname" -g "$group" "$user"
    echo "$user:password" | sudo chpasswd
done < users.txt
```

---

## Review Questions

1. What command modifies an existing user account?

2. What's the difference between `usermod -G` and `usermod -aG`?

3. How do you lock a user account?

4. What does `userdel -r` do?

5. True or False: Deleting a user automatically deletes all their files system-wide.

6. Which command finds files owned by UID 1002?

7. How do you force a user to change their password on next login?

8. What does the `-m` option do with `usermod -d`?

---

## Quick Reference

```
Modify User:
└── usermod [opts] username
    ├── -c "Name"      → Change comment
    ├── -d /path -m    → Move home directory
    ├── -aG group      → Add to group (IMPORTANT: use -a!)
    ├── -G groups      → Replace all supplementary groups
    ├── -L             → Lock account
    ├── -U             → Unlock account
    ├── -s /bin/shell  → Change shell
    └── -e YYYY-MM-DD  → Set expiration

Delete User:
├── userdel user    → Delete (keep home)
└── userdel -r user → Delete (remove home)

Find Orphaned Files:
├── find / -user username -ls  → Before deletion
├── find / -uid 1002 -ls       → After deletion
└── find / -nouser -ls         → All orphaned

Password:
├── passwd user      → Set password
├── passwd -l user   → Lock
├── passwd -u user   → Unlock
└── passwd -e user   → Force change next login
```
