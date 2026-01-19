# SELinux Security Contexts

## What is a Security Context

**Definition:**
*   Classification for objects and subjects
*   Also called security label or ID badge
*   Determines access permissions

**Four attributes:**
1. User
2. Role
3. Type
4. Level

**Format:**
```
user:role:type:level
```

---

## Security Context Attributes

### User Attribute

**Characteristics:**
*   Maps Linux username to SELinux name
*   NOT the same as login name
*   Ends with `_u`

**Examples:**
*   `unconfined_u` - Regular users
*   `system_u` - System processes
*   `root` - Root user

### Role Attribute

**Characteristics:**
*   Maps company role to SELinux role
*   Ends with `_r`
*   Roles authorized for types/domains

**Examples:**
*   `unconfined_r` - Regular user processes
*   `system_r` - System processes
*   `object_r` - Files/objects

### Type Attribute

**Also called:** Security type or domain

**Characteristics:**
*   Domain type for processes
*   User type for users
*   File type for files
*   Ends with `_t`

**Examples:**
*   `unconfined_t` - Unconfined processes
*   `httpd_t` - Apache web server
*   `user_home_t` - User home files

### Level Attribute

**MLS/MCS attribute:**
*   Optional in TE (Type Enforcement)
*   Required in MLS
*   Format: `sensitivity:category`

**Sensitivity:**
*   Security/sensitivity level
*   Hierarchical (s0 = lowest)
*   Range: `s0-s0` or single `s0`

**Category:**
*   Object category
*   Values: c0 to c255
*   Range: `c0.c1023` or single `c0`

---

## User Security Contexts

**View your context:**
```bash
id
```

**Example output:**
```
uid=1000(johndoe) gid=1000(johndoe) groups=1000(johndoe)
context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

**Breakdown:**
*   User: `unconfined_u`
*   Role: `unconfined_r`
*   Type: `unconfined_t`
*   Sensitivity: `s0-s0`
*   Category: `c0.c1023` (all categories)

---

## File Security Contexts

**View file context:**
```bash
ls -Z filename
```

**Example:**
```bash
ls -Z my_stuff
-rw-rw-r--. johndoe johndoe unconfined_u:object_r:user_home_t:s0 my_stuff
```

**Breakdown:**
*   User: `unconfined_u`
*   Role: `object_r`
*   Type: `user_home_t`
*   Level: `s0`

**View directory context:**
```bash
ls -Zd /var/www/html
drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html
```

---

## Process Security Contexts

**View process context:**
```bash
ps -eZ | grep bash
```

**Example output:**
```
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 1589 pts/0 00:00:00 bash
```

**Breakdown:**
*   User: `unconfined_u`
*   Role: `unconfined_r`
*   Type: `unconfined_t` (domain)
*   Sensitivity: `s0-s0`
*   Category: `c0.c1023`

**View specific process:**
```bash
ps -eZ | grep httpd
system_u:system_r:httpd_t:s0 1234 ? 00:00:00 httpd
```

---

## Managing User Contexts

**View user mappings:**
```bash
semanage login -l
```

**Example output:**
```
Login Name    SELinux User    MLS/MCS Range    Service
__default__   unconfined_u    s0-s0:c0.c1023   *
root          unconfined_u    s0-s0:c0.c1023   *
```

**View SELinux users and roles:**
```bash
semanage user -l
```

**Example output:**
```
SELinux User  Prefix  MLS/MCS Level  MLS/MCS Range  SELinux Roles
guest_u       user    s0             s0             guest_r
user_u        user    s0             s0             user_r
```

---

## Managing File Contexts

### Viewing File Contexts

**View with secon:**
```bash
secon -urt -f /etc/passwd
```

**Output:**
```
user: system_u
role: object_r
type: passwd_file_t
```

### Changing File Contexts

**Change user:**
```bash
chcon -u system_u file.txt
```

**Change type:**
```bash
chcon -t httpd_sys_content_t /var/www/html/index.html
```

**Change entire context:**
```bash
chcon -u system_u -r object_r -t httpd_sys_content_t file.txt
```

**Restore default context:**
```bash
restorecon filename
```

**Restore recursively:**
```bash
restorecon -R /var/www/html
```

---

## Setting Persistent File Contexts

**Problem:**
*   `chcon` changes are temporary
*   Lost on relabel

**Solution:** Use `semanage fcontext`

**Add file context rule:**
```bash
semanage fcontext -a -t httpd_sys_content_t "/abc/www/html(/.*)?"
```

**Apply the rule:**
```bash
restorecon -R -v /abc/www/html
```

**View file context rules:**
```bash
semanage fcontext -l | grep /abc
```

---

## Managing Process Contexts

**How processes get contexts:**

**1. Started by systemd:**
*   Assigned specific context
*   Based on service policy
*   Example: httpd gets `httpd_t`

**2. Started by user:**
*   Inherits user's context
*   Runs in user's domain

**3. No policy:**
*   Assigned `unconfined_t`
*   Only DAC restrictions

**Run with specific context:**
```bash
runcon -u user -r role -t type command
```

**Run in sandbox:**
```bash
sandbox command
```

---

## secon Command

**View current process context:**
```bash
secon -urt
```

**View specific process:**
```bash
secon -urt -p PID
```

**View file context:**
```bash
secon -urt -f /path/to/file
```

**Options:**
*   `-u` - Show user
*   `-r` - Show role
*   `-t` - Show type
*   `-s` - Show sensitivity
*   `-c` - Show clearance
*   `-m` - Show MLS range

---

## Common File Types

**Web server:**
*   `httpd_sys_content_t` - Read-only content
*   `httpd_sys_rw_content_t` - Read-write content
*   `httpd_sys_script_exec_t` - CGI scripts

**User files:**
*   `user_home_t` - User home files
*   `user_tmp_t` - User temp files

**System files:**
*   `etc_t` - /etc files
*   `var_t` - /var files
*   `tmp_t` - /tmp files

---

## Review Questions

1. What are the four attributes of a security context?

2. What command shows your security context?

3. How do you view a file's security context?

4. What does the `-Z` option do?

5. What's the difference between `chcon` and `semanage fcontext`?

6. What command restores a file's default context?

7. True or False: Process contexts are always inherited from the parent process.

8. What does the `_t` suffix indicate?

---

## Quick Reference

```
Security Context Format:
└── user:role:type:level

Attributes:
├── User → Ends with _u (unconfined_u, system_u)
├── Role → Ends with _r (unconfined_r, system_r, object_r)
├── Type → Ends with _t (unconfined_t, httpd_t, user_home_t)
└── Level → sensitivity:category (s0:c0.c1023)

View Contexts:
├── id                    → Your context
├── ls -Z file            → File context
├── ls -Zd dir            → Directory context
├── ps -eZ | grep process → Process context
└── secon -urt -f file    → File context details

User Context Management:
├── semanage login -l  → View user mappings
└── semanage user -l   → View SELinux users/roles

File Context Management:
├── chcon -t type file              → Change type (temporary)
├── chcon -u user -r role -t type file → Change all (temporary)
├── restorecon file                 → Restore default
└── restorecon -R dir               → Restore recursively

Persistent File Context:
├── semanage fcontext -a -t type "path(/.*)?" → Add rule
├── restorecon -R -v path                     → Apply rule
└── semanage fcontext -l                      → List rules

Process Context:
├── ps -eZ                → All processes
├── ps -eZ | grep name    → Specific process
├── runcon -t type cmd    → Run with context
└── sandbox cmd           → Run in sandbox

secon Command:
├── secon -urt            → Current process
├── secon -urt -p PID     → Specific process
└── secon -urt -f file    → File context

Common Types:
├── httpd_sys_content_t     → Web content (read-only)
├── httpd_sys_rw_content_t  → Web content (read-write)
├── user_home_t             → User home files
├── etc_t                   → /etc files
└── tmp_t                   → /tmp files

Best Practices:
├── Use semanage fcontext for persistent changes
├── Use restorecon after semanage
├── Test with chcon before making permanent
└── Always verify with ls -Z
```
