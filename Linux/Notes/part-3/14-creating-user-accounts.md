# Creating User Accounts

## Key Concepts

| Term | Definition |
|------|------------|
| **User Account** | Individual login identity with UID, home directory, shell |
| **UID** | User ID number (unique identifier) |
| **GID** | Group ID number |
| **Home Directory** | User's personal directory (typically `/home/username`) |
| **Shell** | Command interpreter assigned to user |

---

## Why Separate User Accounts?

**Security boundaries:**
*   Each user has own files and permissions
*   Users can't access each other's files (by default)
*   Processes run with user's permissions

**Customization:**
*   Each user configures their own environment
*   Different shells, aliases, settings

---

## Creating Users with Cockpit

**Install and enable Cockpit:**
```bash
sudo yum install cockpit -y
sudo systemctl enable --now cockpit.socket
```

**Access:** `http://hostname:9090`

### Creating a User

1. **Log in** as root or privileged user
2. **Select "Accounts"**
3. **Click "Create New Account"**
4. **Fill in fields:**

| Field | Description |
|-------|-------------|
| **Full Name** | Real name (comment field) |
| **User Name** | Login name (lowercase, max 32 chars) |
| **Password** | Set password (8+ chars, complex) |
| **Confirm** | Re-enter password |
| **Access** | Lock account (optional) |

5. **Click "Create"**

### Best Practices for Usernames

✅ **Do:**
*   Use all lowercase
*   Start with a letter
*   Keep it under 8 characters (convention)
*   Use only letters, numbers, underscores

❌ **Don't:**
*   Start with a number (26jsmith)
*   Use uppercase mixed with lowercase (Jsmith vs jsmith confusion)
*   Use spaces or special characters
*   Exceed 32 characters (limit, but causes issues)

---

## Creating Users with useradd

### Basic Syntax

```bash
useradd [options] username
```

### Common Options

| Option | Purpose | Example |
|--------|---------|---------|
| `-c "comment"` | Full name | `-c "John Smith"` |
| `-d home_dir` | Home directory | `-d /home/jsmith` |
| `-e YYYY-MM-DD` | Expiration date | `-e 2025-12-31` |
| `-g group` | Primary group | `-g users` |
| `-G groups` | Supplementary groups | `-G wheel,sales` |
| `-m` | Create home directory | (default in RHEL/Fedora) |
| `-s shell` | Default shell | `-s /bin/bash` |
| `-u uid` | User ID | `-u 1500` |

### Simple Example

```bash
# Create user
sudo useradd -c "Sara Green" sara

# Set password
sudo passwd sara
```

### Advanced Example

```bash
sudo useradd -g users -G wheel,apache -s /bin/tcsh \
  -c "Sara Green" sara
```

**This creates:**
*   Primary group: `users`
*   Supplementary groups: `wheel`, `apache`
*   Shell: `/bin/tcsh`
*   Comment: "Sara Green"

---

## Understanding /etc/passwd

Each user account creates a line in `/etc/passwd`:

```
sara:x:1002:1007:Sara Green:/home/sara:/bin/tcsh
```

### Field Breakdown

| Position | Field | Example | Meaning |
|----------|-------|---------|---------|
| 1 | Username | `sara` | Login name |
| 2 | Password | `x` | Placeholder (real password in /etc/shadow) |
| 3 | UID | `1002` | User ID number |
| 4 | GID | `1007` | Primary group ID |
| 5 | Comment | `Sara Green` | Full name/description |
| 6 | Home | `/home/sara` | Home directory |
| 7 | Shell | `/bin/tcsh` | Default shell |

---

## Understanding /etc/shadow

Encrypted passwords and password policies stored in `/etc/shadow`:

```
sara:$6$ABC123...:18000:0:99999:7:::
```

### Field Breakdown

| Position | Field | Meaning |
|----------|-------|---------|
| 1 | Username | Login name |
| 2 | Encrypted password | Hashed password |
| 3 | Last change | Days since 1970-01-01 |
| 4 | Min days | Days before password can change |
| 5 | Max days | Days before password must change |
| 6 | Warn days | Days warning before expiration |
| 7 | Inactive days | Days after expiration before disable |
| 8 | Expire date | Account expiration date |
| 9 | Reserved | (unused) |

---

## User Defaults

### Configuration Files

**`/etc/login.defs`** - System-wide defaults:
```
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_MIN_LEN    5
PASS_WARN_AGE   7
UID_MIN         1000
UID_MAX         60000
GID_MIN         1000
GID_MAX         60000
CREATE_HOME     yes
```

**`/etc/default/useradd`** - useradd defaults:
```
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

### View Current Defaults

```bash
useradd -D
```

### Change Defaults

```bash
# Change default home base
sudo useradd -D -b /home/users

# Change default shell
sudo useradd -D -s /bin/tcsh

# Change default group
sudo useradd -D -g users
```

---

## Skeleton Directory (/etc/skel)

**Purpose:** Template files copied to new user home directories

**Default location:** `/etc/skel/`

**Contains:**
*   `.bashrc` - Bash configuration
*   `.bash_profile` - Login script
*   `.bash_logout` - Logout script
*   Other default config files

**Customize:**
```bash
# Add file to skeleton
sudo cp mytemplate.txt /etc/skel/

# New users get this file automatically
```

---

## What Happens When useradd Runs?

1. **Reads** `/etc/login.defs` and `/etc/default/useradd`
2. **Checks** command-line options
3. **Creates** entry in `/etc/passwd`
4. **Creates** entry in `/etc/shadow`
5. **Creates** group entry in `/etc/group` (if needed)
6. **Creates** home directory (default: `/home/username`)
7. **Copies** files from `/etc/skel/` to home directory
8. **Sets** ownership and permissions on home directory

---

## UID and GID Ranges

| Range | Purpose |
|-------|---------|
| **0** | Root user |
| **1-199** | System accounts (permanent) |
| **200-999** | Custom system accounts |
| **1000-60000** | Regular users (default) |

---

## Password Best Practices

**Length:** Minimum 8 characters (12+ recommended)

**Complexity:**
*   Mix uppercase and lowercase
*   Include numbers
*   Include punctuation
*   Avoid dictionary words
*   Avoid keyboard patterns (qwerty)
*   Avoid repeated characters

**Generate encrypted password:**
```bash
openssl passwd
```

---

## Special useradd Options

### Don't Create Home Directory

```bash
sudo useradd -M username
```

### Use Custom Skeleton Directory

```bash
sudo useradd -k /path/to/skel -m username
```

### Create User with Same UID (Two Names)

```bash
sudo useradd -o -u 1002 username2
```

### Disable Password Expiration

```bash
sudo useradd -f -1 username
```

---

## Review Questions

1. What command creates a new user account?

2. Where are encrypted passwords stored?

3. What is the default home directory base for new users?

4. What does the UID 0 represent?

5. True or False: Regular user UIDs start at 1000 in RHEL/Fedora.

6. Which directory contains template files for new users?

7. What does the `-G` option do with useradd?

8. How do you set a password for a new user?

---

## Quick Reference

```
Create User:
└── useradd [opts] username → Create user
    ├── -c "Name"     →  Comment/full name
    ├── -d /path      → Home directory
    ├── -g group      → Primary group
    ├── -G grp1,grp2  → Supplementary groups
    ├── -s /bin/bash  → Shell
    └── -u 1500       → User ID

Set Password:
└── passwd username

Key Files:
├── /etc/passwd         → User account info
├── /etc/shadow         → Encrypted passwords
├── /etc/login.defs     → System defaults
├── /etc/default/useradd → useradd defaults
└── /etc/skel/          → Template files

View Defaults:
└── useradd -D
```
