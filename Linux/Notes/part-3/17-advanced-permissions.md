# Advanced Permissions

## Access Control Lists (ACLs)

ACLs extend traditional Unix permissions to allow **fine-grained control** over file/directory access.

**Traditional limitation:** Only one user and one group per file

**ACL solution:** Assign specific permissions to any user or group

---

## ACL Basics

### Key Concepts

*   **Must be enabled** on filesystem at mount time
*   RHEL/Fedora: Auto-enabled on install-time filesystems
*   Use `setfacl` to set ACLs
*   Use `getfacl` to view ACLs
*   **Only file owner** can set ACLs

### ACL Indicators

**Plus sign (+) in ls output:**
```bash
$ ls -l file.txt
-rw-rw-r--+ 1 mary mary 0 Jan 21 09:27 file.txt
```

The `+` means ACLs are set. Use `getfacl` to view them.

---

## Setting ACLs with setfacl

### Syntax

```bash
setfacl -m u:username:perms filename
setfacl -m g:groupname:perms filename
```

### Examples

```bash
# Create file
[mary]$ touch /tmp/memo.txt

# Give bill read/write permission
[mary]$ setfacl -m u:bill:rw /tmp/memo.txt

# Give sales group read/write permission
[mary]$ setfacl -m g:sales:rw /tmp/memo.txt
```

### View ACLs

```bash
[mary]$ getfacl /tmp/memo.txt
# file: tmp/memo.txt
# owner: mary
# group: mary
user::rw-
user:bill:rw-
group::rw-
group:sales:rw-
mask::rw-
other::r--
```

---

## ACL Permissions

| Permission | Symbol | Meaning |
|------------|--------|---------|
| Read | `r` | Read file/list directory |
| Write | `w` | Modify file/create files in directory |
| Execute | `x` | Run file/access directory |
| None | `-` | No permission |

---

## The ACL Mask

**Important:** Once ACLs are set, the **group permission** becomes a **mask**.

**Mask** = Maximum permission any ACL user/group can have

### Example

```bash
[mary]$ chmod 644 /tmp/memo.txt
[mary]$ getfacl /tmp/memo.txt
# file: tmp/memo.txt
user::rw-
user:bill:rw-        #effective:r--
group::rw-           #effective:r--
group:sales:rw-      #effective:r--
mask::r--
other::r--
```

**Result:** Even though bill has `rw-`, effective permission is only `r--` due to mask.

**Fix:** `chmod 664 /tmp/memo.txt` to open the mask.

---

## Removing ACLs

### Remove Specific ACL

```bash
# Remove user ACL
setfacl -x u:bill /tmp/memo.txt

# Remove group ACL
setfacl -x g:sales /tmp/memo.txt
```

### Remove All ACLs

```bash
setfacl -b /tmp/memo.txt
```

---

## Default ACLs

**Default ACLs** on directories are **inherited** by new files/directories.

### Set Default ACL

```bash
[mary]$ mkdir /tmp/mary
[mary]$ setfacl -m d:g:sales:rwx /tmp/mary/
```

The `d:` prefix makes it a default ACL.

### View with getfacl

```bash
[mary]$ getfacl /tmp/mary/
# file: tmp/mary/
user::rwx
group::rwx
other::r-x
default:user::rwx
default:group::rwx
default:group:sales:rwx
default:mask::rwx
default:other::r-x
```

### Test Inheritance

```bash
[mary]$ mkdir /tmp/mary/test
[mary]$ getfacl /tmp/mary/test
# Inherits default ACLs from parent!
```

**Note:** Files inherit ACLs but with execute removed (so effective permission is `rw-`).

---

## Enabling ACLs

### Check if ACLs Enabled (ext4)

```bash
$ mount | grep home
/dev/mapper/vg-home on /home type ext4 (rw)

$ sudo tune2fs -l /dev/mapper/vg-home | grep "mount options"
Default mount options:    user_xattr acl
```

### Enable ACLs (if missing)

**Method 1: Implant in filesystem**
```bash
sudo tune2fs -o acl /dev/sdc1
```

**Method 2: Add to /etc/fstab**
```
/dev/sdc1  /var/stuff  ext4  acl  1 2
```

**Method 3: Manual mount**
```bash
sudo mount -o acl /dev/sdc1 /var/stuff
```

---

## Set GID Bit Directories

**Purpose:** Files created in directory inherit the directory's group.

**Use case:** Team collaboration directory

### Create Set GID Directory

```bash
# Create group
sudo groupadd sales

# Create directory
sudo mkdir /mnt/salestools

# Assign group
sudo chgrp sales /mnt/salestools

# Set permissions + set GID bit
sudo chmod 2775 /mnt/salestools
```

The `2` in `2775` enables the set GID bit.

### Verify

```bash
$ ls -ld /mnt/salestools
drwxrwsr-x. 2 root sales 4096 Jan 22 14:32 /mnt/salestools/
```

Notice `s` where group execute bit normally is: `rwxrwsr-x`

### Test It

```bash
[mary]$ touch /mnt/salestools/test.txt
[mary]$ ls -l /mnt/salestools/test.txt
-rw-rw-r--. 1 mary sales 0 Jan 22 14:32 test.txt
```

File is owned by `mary` but **group is `sales`** (inherited from directory).

---

## Sticky Bit Directories

**Purpose:** Only owner can delete files (even if directory is world-writable).

**Use case:** `/tmp` directory

### What's Different?

**Normal directory:**
*   If you have write permission, you can delete anyone's files

**Sticky bit directory:**
*   Even with write permission, you can only delete **your own** files
*   Root and directory owner can delete any file

### Example: /tmp

```bash
$ ls -ld /tmp
drwxrwxrwt. 116 root root 36864 Jan 22 14:18 /tmp
```

Notice `t` instead of `x` in other execute: `drwxrwxrwt`

### Create Sticky Bit Directory

```bash
# Create directory
mkdir /tmp/mystuff

# Set permissions + sticky bit
chmod 1777 /tmp/mystuff
```

The `1` in `1777` enables the sticky bit.

### Test It

```bash
[mary]$ touch /tmp/mystuff/maryfile.txt
[mary]$ chmod 666 /tmp/mystuff/maryfile.txt

[bill]$ rm /tmp/mystuff/maryfile.txt
rm: cannot remove '/tmp/mystuff/maryfile.txt': Operation not permitted
```

Even though the file is `666` (world-writable), bill can't delete it.

---

## Special Permission Bits Summary

| Bit | Numeric | Letter | Purpose |
|-----|---------|--------|---------|
| **Set UID** | 4 | `u+s` | Run as file owner |
| **Set GID** | 2 | `g+s` | Run as file group / Inherit directory group |
| **Sticky** | 1 | `o+t` | Restricted deletion |

### Set with chmod

**Numeric:**
```bash
chmod 2775 dir    # Set GID + rwxrwxr-x
chmod 1777 dir    # Sticky + rwxrwxrwx
chmod 4755 file   # Set UID + rwxr-xr-x
```

**Symbolic:**
```bash
chmod g+s dir     # Set GID bit
chmod o+t dir     # Sticky bit
chmod u+s file    # Set UID bit
```

---

## Centralized User Authentication

For enterprise environments with hundreds/thousands of systems.

### Supported Methods

**LDAP (Lightweight Directory Access Protocol):**
*   Open standard
*   Centralized user database
*   Used in many enterprises

**NIS (Network Information Service):**
*   Older Sun Microsystems protocol
*   Less secure (clear text)
*   Mostly replaced by LDAP

**Winbind (Active Directory):**
*   Microsoft Active Directory integration
*   Common in Windows-heavy enterprises
*   RHEL/Fedora can authenticate against AD

### Configuration

Use `authconfig` or `authselect` to configure centralized authentication.

**Example (conceptual):**
```bash
# Configure LDAP authentication
sudo authconfig --enableldap \
  --enableldapauth \
  --ldapserver=ldap://ldap.example.com \
  --ldapbasedn="dc=example,dc=com" \
  --update
```

### Recommended Open-Source Server

**389 Directory Server:**
*   Enterprise LDAP server
*   Available for Fedora/RHEL
*   https://directory.fedoraproject.org/

---

## Review Questions

1. What command sets ACLs on a file?

2. How do you view ACLs on a file?

3. What does the + sign in `ls -l` output indicate?

4. True or False: The ACL mask limits maximum permissions for ACL users/groups.

5. What does the set GID bit do on a directory?

6. What does the sticky bit do on a directory?

7. Which special permission bit is represented by the number 2?

8. Name two centralized authentication protocols supported by Linux.

---

## Quick Reference

```
ACLs:
├── setfacl -m u:user:rwx file   → Set user ACL
├── setfacl -m g:group:rw file   → Set group ACL
├── setfacl -m d:g:group:rwx dir → Set default ACL
├── setfacl -x u:user file       → Remove user ACL
├── setfacl -b file              → Remove all ACLs
└── getfacl file                 → View ACLs

Special Bits (numeric):
├── chmod 4755 file   → Set UID (4)
├── chmod 2775 dir    → Set GID (2)
└── chmod 1777 dir    → Sticky (1)

Special Bits (symbolic):
├── chmod u+s file    → Set UID
├── chmod g+s dir     → Set GID
└── chmod o+t dir     → Sticky

Special Bit Indicators (ls -l):
├── rws------  → Set UID (s in user execute)
├── ---rws---  → Set GID (s in group execute)
└── ------rwt  → Sticky (t in other execute)

Set GID Directory:
└── Files inherit directory's group

Sticky Bit Directory:
└── Only owner can delete files
```
