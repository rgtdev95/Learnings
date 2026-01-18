# System Administration Basics

## Key Concepts

| Term | Definition |
|------|------------|
| **System Administrator** | Person responsible for managing all system resources |
| **root User** | Superuser account with complete system control (UID 0) |
| **Multiuser System** | Multiple people can have accounts with isolated data |
| **Multitasking** | Multiple programs run simultaneously |

---

## Why Separate Administration?

Linux separates administrative tasks from regular user tasks for several reasons:

1. **Security:** Prevents unauthorized system changes
2. **Stability:** Reduces accidental system damage
3. **Accountability:** Tracks who performs administrative tasks
4. **Protection:** Users can't casually harm the system while doing regular work

---

## The root User

The **root** user (also called **superuser**) has **complete control** over the system.

**Root User Properties:**
*   **User ID (UID):** 0
*   **Group ID (GID):** 0 (root group)
*   **Home Directory:** `/root`
*   **Default Shell:** `/bin/bash`

**Root Entry in `/etc/passwd`:**
```
root:x:0:0:root:/root:/bin/bash
```

> **Warning:** With great power comes great responsibility. The root user can delete any file, kill any process, and access all data on the system.

---

## Administrative Responsibilities

Tasks that typically require root privilege include:

| Task | Why Root is Needed |
|------|-------------------|
| **Filesystems** | Mounting/unmounting, accessing any user's files, backups |
| **Software Installation** | Installing system-wide software, preventing malicious code |
| **User Accounts** | Creating/removing users and groups |
| **Network Interfaces** | Configuring network settings (though some desktops allow users) |
| **Servers** | Starting/stopping/configuring services (httpd, sshd, etc.) |
| **Security** | Setting up firewalls, monitoring services, managing access |

---

## Service User Accounts

Linux uses special administrative accounts to run services with limited privileges:

| User | Purpose |
|------|---------|
| `apache` / `www-data` | Runs web server (httpd/apache2) |
| `lp` | Owns printing files and processes |
| `postfix` | Runs mail server processes |
| `avahi` | Provides zeroconf network services |
| `chrony` | Maintains system time |
| `rpc` | Handles remote procedure calls |

**Why?** If a service is compromised, the attacker only has that service user's limited permissions, not full root access.

---

## Review Questions

1. What is the UID of the root user?

2. Why is system administration separated from regular user tasks?

3. Where is the root user's home directory located?

4. Name three tasks that require root privilege.

5. True or False: Regular users can install system-wide software by default.

6. What user typically runs the Apache web server on RHEL/Fedora?

7. Why do services run under separate user accounts instead of root?

8. Can a regular user mount a USB drive on a modern GNOME desktop?

---

## Quick Reference

```
Root User:
├── UID: 0
├── Home: /root
└── Shell: /bin/bash

Administrative Tasks:
├── Filesystem management
├── Software installation
├── User/group management
├── Network configuration
├── Service management
└── Security configuration
```
