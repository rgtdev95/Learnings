# Managing Groups

## Understanding Group Accounts

Groups provide a way to assign permissions to multiple users at once.

**Purpose:**
*   Share files among team members
*   Organize users by role/department
*   Simplify permission management

---

## Types of Groups

### Primary Group

**Definition:** Default group assigned to new files/directories created by user

**Assignment:** Set in `/etc/passwd` (4th field)

**Example:**
```
sara:x:1002:1007:Sara Green:/home/sara:/bin/bash
```
Here, `1007` is sara's primary group ID.

### Supplementary (Secondary) Groups

**Definition:** Additional groups a user belongs to

**Purpose:** Grant access to shared resources

**List:** Shown in `/etc/group` file

---

## Understanding /etc/group

Each group has an entry in `/etc/group`:

```
sales:x:1302:joe,bill,sally,sara
```

### Field Breakdown

| Position | Field | Example | Meaning |
|----------|-------|---------|---------|
| 1 | Group name | `sales` | Name of group |
| 2 | Password | `x` | Group password (rarely used) |
| 3 | GID | `1302` | Group ID number |
| 4 | Members | `joe,bill,sara` | Comma-separated user list |

---

## Group Permissions in Action

```bash
$ ls -ld /var/salesdocs /var/salesdocs/file.txt
drwxrwxr-x. 2 root sales 4096 Jan 14 09:32 /var/salesdocs/
-rw-rw-r--. 1 root sales    0 Jan 14 09:32 /var/salesdocs/file.txt
```

**Breakdown:**
*   Directory: `drwxrwxr-x` - Group `sales` has `rwx`
*   File: `-rw-rw-r--` - Group `sales` has `rw-`

**Result:** Any member of `sales` group can:
*   Read, create, delete files in `/var/salesdocs/` (directory `rwx`)
*   Read and modify `file.txt` (file `rw-`)

---

## Creating Groups with groupadd

### Basic Syntax

```bash
sudo groupadd groupname
```

### Specify GID

```bash
sudo groupadd -g 1325 jokers
```

### Examples

```bash
# Create group with next available GID
sudo groupadd developers

# Create with specific GID
sudo groupadd -g 5000 sysadmins

# Create system group (GID < 1000)
sudo groupadd -r services
```

---

## GID Ranges

| Range | Purpose |
|-------|---------|
| **0** | Root group |
| **1-199** | System groups (permanent) |
| **200-999** | Custom system groups |
| **1000-60000** | Regular groups |

---

## Modifying Groups with groupmod

### Change GID

```bash
sudo groupmod -g 330 jokers
```

### Rename Group

```bash
sudo groupmod -n jacks jokers
```

Changes group name from `jokers` to `jacks`.

---

## Deleting Groups with groupdel

```bash
sudo groupdel groupname
```

**Note:** Can't delete a group if it's the primary group of any user.

---

## Assigning Users to Groups

### During User Creation

```bash
# Set primary group
sudo useradd -g developers alice

# Set primary + supplementary groups
sudo useradd -g developers -G wheel,docker alice
```

### After User Creation

```bash
# Add to supplementary groups
sudo usermod -aG sales,marketing sara

# Change primary group
sudo usermod -g newgroup sara

# Replace ALL supplementary groups
sudo usermod -G wheel,docker sara
```

---

## Temporary Group Switching with newgrp

Users can temporarily change their primary group.

### Basic Usage

```bash
[sara]$ touch file1
[sara]$ newgrp sales
[sara]$ touch file2
[sara]$ ls -l file*
-rw-rw-r--. 1 sara sara  0 Jan 18 22:22 file1
-rw-rw-r--. 1 sara sales 0 Jan 18 22:23 file2
[sara]$ exit
```

**What happened:**
*   `file1` created with primary group `sara`
*   After `newgrp sales`, `file2` created with group `sales`
*   `exit` returns to original group

---

## Group Passwords

**Rarely used**, but possible to allow temporary group membership.

### Set Group Password

```bash
sudo gpasswd sales
```

### Use Without Being Member

```bash
$ newgrp sales
Password: <enter group password>
```

Now you temporarily have `sales` as primary group.

---

## Viewing Group Membership

### For Current User

```bash
groups
```

### For Specific User

```bash
groups username
```

### User's Primary Group

```bash
id -gn username
```

### All Groups for User

```bash
id -Gn username
```

### Detailed ID Information

```bash
id username
```

**Output:**
```
uid=1002(sara) gid=1007(sara) groups=1007(sara),10(wheel),1302(sales),1303(marketing)
```

---

## Managing Group Membership with gpasswd

### Add User to Group

```bash
sudo gpasswd -a username groupname
```

### Remove User from Group

```bash
sudo gpasswd -d username group name
```

### Set Group Administrators

```bash
sudo gpasswd -A alice,bob developers
```

Allows alice and bob to add/remove members from `developers` group.

---

## Common Group Scenarios

### Sales Team Shared Directory

```bash
# Create group
sudo groupadd sales

# Add users
sudo usermod -aG sales alice
sudo usermod -aG sales bob
sudo usermod -aG sales charlie

# Create shared directory
sudo mkdir /shared/sales
sudo chgrp sales /shared/sales
sudo chmod 2775 /shared/sales
```

The `2` enables set GID bit (covered in advanced permissions).

---

## Important Groups in Linux

| Group | Purpose |
|-------|---------|
| **root** | Superuser group (GID 0) |
| **wheel** | Administrative access (sudo) |
| **users** | Default group for regular users |
| **adm** | System monitoring logs |
| **sys** | System files |
| **disk** | Direct disk access |
| **docker** | Docker daemon access |

---

## Review Questions

1. What is a user's primary group?

2. Where is group information stored?

3. What command creates a new group?

4. How do you add a user to a supplementary group?

5. True or False: A user can belong to multiple groups.

6. What command temporarily changes your primary group?

7. What does `groups alice` show?

8. How do you rename a group?

---

## Quick Reference

```
Create Group:
└── groupadd [opts] groupname
    ├── -g 1500        → Specify GID
    └── -r             → System group

Modify Group:
├── groupmod -g 330 group    → Change GID
└── groupmod -n newname old  → Rename

Delete Group:
└── groupdel groupname

Assign Users:
├── useradd -g primary -G supp1,supp2 user → At creation
├── usermod -g newprimary user → Change primary
└── usermod -aG group1,group2 user → Add supplementary

View Membership:
├── groups             → Current user's groups
├── groups username    → User's groups
├── id username        → Detailed info
└── id -Gn username    → Group names only

Group Management:
├── gpasswd -a user group  → Add user to group
├── gpasswd -d user group  → Remove user from group
└── newgrp groupname       → Temporary group switch

Key Files:
└── /etc/group         → Group database
```
