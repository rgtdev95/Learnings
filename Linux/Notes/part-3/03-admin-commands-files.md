# Administrative Commands, Config Files, and Logs

## Administrative Command Directories

Commands intended for system administrators are stored in specific directories.

| Directory | Contents | Access |
|-----------|----------|--------|
| `/sbin` | System binaries (boot, filesystem tools) | Symlink to `/usr/sbin` |
| `/usr/sbin` | User system binaries (user management, daemons) | Added to root's PATH |

**Examples:**
*   `/sbin`: `fsck`, `swapon`, `fdisk`
*   `/usr/sbin`: `useradd`, `sshd`, `cupsd`

> **Note:** In modern RHEL/Fedora/Ubuntu, `/sbin` → `/usr/sbin` (symlink). Both root and regular users have `/usr/sbin` in their PATH.

### Other Admin Command Locations
*   `/bin`, `/usr/bin`: Some commands available to all (e.g., `mount`)
*   `/usr/local/sbin`: Third-party or custom admin tools
*   `/opt/bin`: Optional software packages

---

## Configuration Files

Most Linux configuration is stored in **plain-text files**.

**Advantages:**
*   Easy to read and edit with any text editor
*   Can be version-controlled

**Disadvantages:**
*   No automatic error checking (syntax errors break services)
*   Each file has its own structure/syntax

### Main Configuration Locations

#### `/etc` - System-Wide Configuration
```
/etc/
├── passwd              → User account information
├── shadow              → Encrypted passwords
├── group               → Group definitions
├── fstab               → Filesystem mount table
├── hostname            → System hostname
├── hosts               → Local hostname-to-IP mappings
├── sudoers             → sudo permissions
├── rsyslog.conf        → Logging configuration
├── crontab             → Scheduled tasks
└── ...
```

#### Common `/etc` Subdirectories
*   `/etc/systemd/` - systemd service configurations
*   `/etc/sysconfig/` - RHEL/Fedora service settings
*   `/etc/security/` - PAM authentication settings
*   `/etc/skel/` - Default files for new users
*   `/etc/httpd/` - Apache web server config
*   `/etc/cups/` - Printing service config

#### `$HOME` - User-Specific Configuration
*   `.bashrc` - Per-user bash settings
*   `.ssh/` - SSH keys and config
*   `.vimrc` - Vim editor settings
*   Hidden files (start with `.`)

---

## Log Files and Logging

Linux tracks system activity through logging. There are two main logging systems:

### 1. systemd Journal (journalctl)
Modern logging system used by systemd.

**View All Logs:**
```bash
journalctl
```

**View Specific Boot:**
```bash
journalctl --list-boots
journalctl -b 0    # Current boot
journalctl -b -1   # Previous boot
```

**Filter by Service:**
```bash
journalctl -u sshd.service
journalctl -u httpd.service
```

**Follow Live Messages:**
```bash
journalctl -f
```

**Filter by Priority (0-7):**
```bash
journalctl PRIORITY=0   # Emergency only
journalctl PRIORITY=3   # Errors and above
```

### 2. rsyslogd (Traditional Logging)
Older logging daemon that writes to files in `/var/log/`.

**Common Log Files:**
| File | Contents |
|------|----------|
| `/var/log/messages` | General system messages |
| `/var/log/secure` | Authentication and security events |
| `/var/log/boot.log` | Boot messages |
| `/var/log/cron` | Cron job execution |

**Configuration:** `/etc/rsyslog.conf`

---

## Review Questions

1. Where are most administrative commands stored?

2. What is the main system-wide configuration directory?

3. Which file contains user account information?

4. Which command views the systemd journal?

5. How do you follow journal messages in real time?

6. Where does rsyslogd typically store log files?

7. True or False: Configuration files in Linux are usually binary files.

8. Which file defines which filesystems are mounted at boot?

---

## Quick Reference

```
Commands:
├── /usr/sbin        → Admin binaries
└── /usr/local/sbin  → Custom admin tools

Config Files:
├── /etc/            → System-wide configs
├── /etc/passwd      → User accounts
├── /etc/fstab       → Mount table
└── $HOME/.bashrc    → User bash config

Logs:
├── journalctl       → View systemd journal
├── journalctl -f    → Follow logs
└── /var/log/        → Traditional log files
```
