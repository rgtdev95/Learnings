# Gaining Root Access

## Key Concepts

| Method | Description |
|--------|-------------|
| **Direct Login** | Log in as root (not recommended for daily use) |
| **su** | Switch user to become root temporarily |
| **sudo** | Execute single commands with root privilege |
| **Cockpit** | Browser-based administration interface |

---

## Method 1: Direct Login as root

You can log in directly as the root user, but this is **not recommended** for daily use.

**To try it safely:**
1. Press `Ctrl+Alt+F3` to get a virtual console
2. Login as `root` with the root password
3. Type `exit` when finished
4. Press `Ctrl+Alt+F1` to return to desktop

> **Ubuntu Note:** By default, root login is disabled. You must use `sudo` instead.

---

## Method 2: su Command

Switch to root from a regular user shell.

### Basic su
```bash
$ su
Password: ******
#
```
*   Prompts for the **root password**
*   Changes prompt from `$` to `#`
*   **Limitation:** Doesn't load root's environment

### su with dash (Recommended)
```bash
$ su -
Password: ******
#
```
*   Loads root user's full environment (PATH, etc.)
*   Changes to `/root` directory
*   **Best practice for becoming root**

### Switching to Other Users
```bash
$ su - jsmith
```
*   As root, no password needed
*   As regular user, you need jsmith's password

**Exit:** Press `Ctrl+D` or type `exit`

---

## Method 3: sudo Command

Allows authorized users to run commands with root privilege **using their own password**.

### Basic Usage
```bash
$ sudo touch /mnt/testfile.txt
[sudo] password for joe: *********
$ ls -l /mnt/testfile.txt
-rw-r--r--. 1 root root 0 Jan 7 08:42 /mnt/testfile.txt
```

**Benefits:**
*   Users don't need the root password
*   All administrative commands are logged
*   Privilege lasts 5 minutes (configurable)
*   Fine-grained control over what users can do

---

## Configuring sudo (sudoers)

Edit `/etc/sudoers` using the **visudo** command (never edit directly!).

### Give Full Root Privilege
```bash
# visudo
```
Add line:
```
joe  ALL=(ALL)  ALL
```

### Give Privilege Without Password
```
joe  ALL=(ALL)  NOPASSWD: ALL
```

### How It Works
*   `joe` = username
*   `ALL` = from any host
*   `(ALL)` = as any user
*   `ALL` = run any command

---

## Method 4: Cockpit Web Interface

Browser-based system administration tool (RHEL 8+, Fedora, Ubuntu).

### Enable Cockpit
```bash
# dnf install cockpit              # If not installed
# systemctl enable --now cockpit.socket
```

### Access Cockpit
Open browser to: `https://hostname:9090`
*   Default port: 9090 (HTTPS)
*   Login with root or sudo-enabled user
*   Accept self-signed certificate warning

**Cockpit Features:**
*   System monitoring (CPU, memory, disk, network)
*   Service management
*   Storage management
*   User/group accounts
*   Networking configuration
*   Log viewing

---

## Review Questions

1. Which `su` command loads the root environment properly?

2. What is the main advantage of `sudo` over `su`?

3. Which command do you use to edit the sudoers file?

4. On what port does Cockpit run by default?

5. True or False: After typing `sudo`, you enter the root password.

6. How long does sudo privilege last by default?

7. What does `visudo` do that makes it safer than editing `/etc/sudoers` directly?

8. To exit a root shell started with `su`, what do you type?

---

## Quick Reference

```
Gaining Root:
├── su           → Switch to root (minimal env)
├── su -         → Switch to root (full env)
├── sudo cmd     → Run single command as root
└── visudo       → Edit sudoers file

Cockpit:
├── Install: dnf install cockpit
├── Enable: systemctl enable --now cockpit.socket
└── Access: https://hostname:9090
```
