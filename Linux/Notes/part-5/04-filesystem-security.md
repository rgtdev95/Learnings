# Filesystem Security

## Managing SUID and SGID Permissions

### Understanding SUID (Set User ID)

**What it does:**
*   Allows user to run file as file's owner
*   Temporary privilege escalation
*   Only while file executes in memory

**Permission notation:**
*   `rwsr-xr-x` - SUID bit set (s in owner execute)
*   Octal: 4755

**Risk:**
*   If owner is root, anyone becomes root temporarily
*   Favorite target of malicious users
*   Use sparingly

**Legitimate examples:**
```bash
$ ls -l /usr/bin/passwd
-rwsr-xr-x. 1 root root 28804 Aug 17 20:50 /usr/bin/passwd

$ ls -l /usr/bin/sudo
---s--x--x. 2 root root 77364 Nov 3 08:10 /usr/bin/sudo
```

**Why these need SUID:**
*   `passwd` - Must write to /etc/shadow (root-owned)
*   `sudo` - Must verify sudoers and escalate privileges

### Understanding SGID (Set Group ID)

**What it does:**
*   Allows user to run file as file's group
*   Temporary group membership
*   Only while file executes

**On directories:**
*   Files created inherit directory's group
*   Not creator's default group

**Permission notation:**
*   `rwxr-sr-x` - SGID bit set (s in group execute)
*   Octal: 2755

---

## Finding SUID and SGID Files

**Find all SUID/SGID files:**
```bash
find / -perm /6000 -ls
```

**Example output:**
```
4597316 52 -rwxr-sr-x 1 root games  51952 Dec 21 2013 /usr/bin/atc
4589119 20 -rwxr-sr-x 1 root tty    19552 Nov 18 2013 /usr/bin/write
4587931 60 -rwsr-xr-x 1 root root   57888 Aug  2 2013 /usr/bin/at
4588045 60 -rwsr-xr-x 1 root root   57536 Sep 25 2013 /usr/bin/crontab
4588961 32 -rwsr-xr-x 1 root root   32024 Nov 18 2013 /usr/bin/su
5767487 85 -rwsrwsr-x 1 root root   68928 Sep 13 11:52 /var/.bin/myvi
```

**Red flag:**
*   `/var/.bin/myvi` - Hidden directory with SUID
*   Copy of vi with root ownership
*   Can edit any file as root
*   Malicious user activity

**Find only SUID:**
```bash
find / -perm -4000 -ls
```

**Find only SGID:**
```bash
find / -perm -2000 -ls
```

---

## Securing Password Files

### /etc/passwd

**Required permissions:**
*   Owner: root
*   Group: root
*   Permissions: 644 (rw-r--r--)

**Verify:**
```bash
ls -l /etc/passwd
-rw-r--r--. 1 root root 1644 Feb  2 02:30 /etc/passwd
```

**Why these permissions:**
*   Users need read access to log in
*   See usernames for UID/GID numbers
*   Cannot modify directly

### /etc/shadow

**Required permissions:**
*   Owner: root
*   Group: root
*   Permissions: 000 (----------)

**Verify:**
```bash
ls -l /etc/shadow
----------. 1 root root 1049 Feb  2 09:45 /etc/shadow
```

**Why these permissions:**
*   Contains hashed passwords
*   Only root can read
*   Much more secure than /etc/passwd

**How users change passwords:**
*   `passwd` command has SUID bit
*   Temporarily runs as root
*   Can write to /etc/shadow

### /etc/group

**Required permissions:**
*   Owner: root
*   Group: root
*   Permissions: 644 (rw-r--r--)

**Same as /etc/passwd:**
*   Users need read access
*   See group information
*   Cannot modify directly

### /etc/gshadow

**Required permissions:**
*   Owner: root
*   Group: root
*   Permissions: 000 (----------)

**Same as /etc/shadow:**
*   Contains group passwords
*   Only root can read
*   Highly restricted

---

## Securing /etc/fstab

**Required permissions:**
*   Owner: root
*   Group: root
*   Permissions: 644 (rw-r--r--)

**Verify:**
```bash
ls -l /etc/fstab
-rw-r--r--. 1 root root 465 Feb  2 02:30 /etc/fstab
```

---

## Filesystem Mount Options

### /home Partition

**Recommended options:**
```
/dev/sdb1  /home  ext4  defaults,nodev,noexec,nosuid  1 2
```

**Option explanations:**

**nosuid:**
*   Prevents SUID/SGID programs from running
*   Programs needing SUID shouldn't be in /home
*   Likely malicious if found there

**nodev:**
*   Device files not recognized
*   Device files belong in /dev
*   Not in /home

**noexec:**
*   Executable programs cannot run from /home
*   Prevents running malicious executables
*   Scripts can still run (interpreted)

### /tmp Partition

**Recommended options:**
```
/dev/sdc1  /tmp  ext4  defaults,nodev,noexec,nosuid  1 1
```

**Same options as /home:**
*   `nosuid` - No SUID/SGID execution
*   `nodev` - No device files
*   `noexec` - No executables

**Why:**
*   Temporary files shouldn't be executable
*   Common attack vector
*   World-writable directory

### /usr Partition

**Recommended options:**
```
/dev/sdb2  /usr  ext4  defaults,nodev  1 2
```

**Option:**
*   `nodev` - No device files recognized

**Why:**
*   Contains user programs and data
*   Little change after software installation
*   Sometimes mounted read-only
*   Device files don't belong here

### /var Partition

**Recommended options:**
```
/dev/sdb3  /var  ext4  defaults,nodev,noexec,nosuid  1 2
```

**Same options as /home and /tmp:**
*   `nosuid` - No SUID/SGID execution
*   `nodev` - No device files
*   `noexec` - No executables

**Why:**
*   Grows with logs and server content
*   Should be separate partition on servers
*   Prevents filling root filesystem

---

## Complete /etc/fstab Example

```
# Device        Mount   Type  Options                        Dump Pass
/dev/sda1       /boot   ext4  defaults                       1    2
/dev/sda2       /       ext4  defaults                       1    1
/dev/sda3       swap    swap  defaults                       0    0
/dev/sdb1       /home   ext4  defaults,nodev,noexec,nosuid   1    2
/dev/sdc1       /tmp    ext4  defaults,nodev,noexec,nosuid   1    1
/dev/sdb2       /usr    ext4  defaults,nodev                 1    2
/dev/sdb3       /var    ext4  defaults,nodev,noexec,nosuid   1    2
```

---

## Review Questions

1. What does the SUID permission do?

2. What command finds all files with SUID or SGID set?

3. What should the permissions be for /etc/shadow?

4. How can users change their password if /etc/shadow has no permissions?

5. What mount option prevents SUID/SGID programs from running?

6. What mount option prevents executables from running?

7. True or False: Device files should be stored in /home.

8. What partitions should have the noexec option?

---

## Quick Reference

```
SUID/SGID:
├── SUID → Run as file owner (s in owner execute)
├── SGID → Run as file group (s in group execute)
├── Octal: 4755 (SUID), 2755 (SGID), 6755 (both)
└── Risk: Privilege escalation

Find SUID/SGID:
├── find / -perm /6000 -ls  → Both
├── find / -perm -4000 -ls  → SUID only
└── find / -perm -2000 -ls  → SGID only

File Permissions:
├── /etc/passwd  → -rw-r--r-- (644) root:root
├── /etc/shadow  → ---------- (000) root:root
├── /etc/group   → -rw-r--r-- (644) root:root
├── /etc/gshadow → ---------- (000) root:root
└── /etc/fstab   → -rw-r--r-- (644) root:root

Mount Options:
├── nosuid → No SUID/SGID execution
├── nodev  → No device files recognized
├── noexec → No executables can run
└── defaults → Default options

Partition Security:
├── /home → nodev,noexec,nosuid
├── /tmp  → nodev,noexec,nosuid
├── /usr  → nodev
└── /var  → nodev,noexec,nosuid

/etc/fstab Format:
Device  Mount  Type  Options  Dump  Pass

Example Entry:
/dev/sdb1  /home  ext4  defaults,nodev,noexec,nosuid  1  2

Legitimate SUID Programs:
├── /usr/bin/passwd → Change passwords
├── /usr/bin/sudo   → Privilege escalation
├── /usr/bin/su     → Switch user
└── /usr/bin/crontab → Edit cron jobs

Security Best Practices:
├── Minimize SUID/SGID files
├── Regular scans for unauthorized SUID
├── Secure password file permissions
├── Use mount options to restrict partitions
├── Separate partitions for /home, /tmp, /var
└── Monitor for hidden SUID files
```
