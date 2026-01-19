# vsftpd Installation

## Installing vsftpd

### Fedora/RHEL

**Install package:**
```bash
yum install vsftpd
```

**Check package info:**
```bash
rpm -qi vsftpd
```

### Ubuntu/Debian

**Install package:**
```bash
apt-get install vsftpd
```

---

## Package Contents

**Main daemon:**
*   `/usr/sbin/vsftpd`

**Configuration file:**
*   `/etc/vsftpd/vsftpd.conf` (RHEL/Fedora)
*   `/etc/vsftpd.conf` (Ubuntu/Debian)

**User lists:**
*   `/etc/vsftpd/ftpusers` - Denied users
*   `/etc/vsftpd/user_list` - User access control

**Service file:**
*   `/usr/lib/systemd/system/vsftpd.service`

**Man pages:**
*   `man vsftpd`
*   `man vsftpd.conf`

---

## Starting vsftpd

### RHEL 7/8 and Fedora

**Enable at boot:**
```bash
systemctl enable vsftpd.service
```

**Start service:**
```bash
systemctl start vsftpd.service
```

**Check status:**
```bash
systemctl status vsftpd.service
```

**Example output:**
```
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled)
   Active: active (running) since Mon 2019-09-02 16:30:00 EDT
   Main PID: 12345 (vsftpd)
```

**Restart:**
```bash
systemctl restart vsftpd.service
```

**Reload config:**
```bash
systemctl reload vsftpd.service
```

### RHEL 6 and Earlier

**Enable at boot:**
```bash
chkconfig vsftpd on
```

**Start service:**
```bash
service vsftpd start
```

**Check status:**
```bash
service vsftpd status
```

---

## Default Configuration

**Default settings:**
*   Anonymous FTP: Disabled
*   Local users: Enabled
*   Write access: Disabled
*   Chroot: Disabled
*   Port: 21

**Default FTP root:**
*   Anonymous: `/var/ftp`
*   Local users: Their home directories

---

## Testing vsftpd

**Check if running:**
```bash
systemctl status vsftpd.service
```

**Check listening port:**
```bash
netstat -tupln | grep vsftpd
```

**Expected output:**
```
tcp6  0  0  :::21  :::*  LISTEN  12345/vsftpd
```

**Test connection:**
```bash
ftp localhost
```

**Example session:**
```
Connected to localhost.
220 (vsFTPd 3.0.3)
Name (localhost:user): testuser
331 Please specify the password.
Password:
230 Login successful.
ftp> quit
221 Goodbye.
```

---

## vsftpd Process Model

**Standalone mode (default):**
*   vsftpd daemon runs continuously
*   Listens on port 21
*   Spawns process for each connection

**xinetd mode (alternative):**
*   xinetd listens on port 21
*   Starts vsftpd when connection arrives
*   Less resource usage when idle

---

## Default Directory Structure

**Anonymous FTP:**
```
/var/ftp/
├── pub/          # Public files
└── incoming/     # Upload directory (if enabled)
```

**Local users:**
*   Home directory: `/home/username`
*   Can access entire home directory (unless chroot enabled)

---

## Configuration File Location

**RHEL/Fedora:**
```
/etc/vsftpd/vsftpd.conf
```

**Ubuntu/Debian:**
```
/etc/vsftpd.conf
```

**Backup before editing:**
```bash
cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
```

---

## User Access Files

### ftpusers

**Location:** `/etc/vsftpd/ftpusers`

**Purpose:** List of users DENIED FTP access

**Default contents:**
```
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
```

**⚠️ Important:** root is denied by default for security

### user_list

**Location:** `/etc/vsftpd/user_list`

**Purpose:** Access control list

**Behavior depends on config:**
*   `userlist_deny=YES` - Users in list are DENIED
*   `userlist_deny=NO` - Only users in list are ALLOWED

**Default:** Deny mode (same as ftpusers)

---

## Log Files

**Transfer log:**
*   `/var/log/xferlog`
*   Records file transfers

**vsftpd log:**
*   `/var/log/vsftpd.log`
*   General vsftpd messages

**View logs:**
```bash
tail -f /var/log/vsftpd.log
tail -f /var/log/xferlog
```

---

## Review Questions

1. What package provides the vsftpd server?

2. What is the default FTP port?

3. Where is the vsftpd configuration file located in RHEL?

4. What command starts vsftpd in RHEL 7+?

5. True or False: Anonymous FTP is enabled by default.

6. What file lists users denied FTP access?

7. Where is the default anonymous FTP root?

8. How do you check if vsftpd is listening on port 21?

---

## Quick Reference

```
Installation:
└── yum install vsftpd

Configuration:
├── RHEL/Fedora: /etc/vsftpd/vsftpd.conf
└── Ubuntu/Debian: /etc/vsftpd.conf

User Lists:
├── /etc/vsftpd/ftpusers   → Denied users
└── /etc/vsftpd/user_list  → Access control

Service (RHEL 7+):
├── systemctl enable vsftpd.service  → Enable at boot
├── systemctl start vsftpd.service   → Start
├── systemctl status vsftpd.service  → Check status
└── systemctl restart vsftpd.service → Restart

Service (RHEL 6):
├── chkconfig vsftpd on  → Enable at boot
├── service vsftpd start → Start
└── service vsftpd status → Check status

Default Settings:
├── Anonymous FTP: Disabled
├── Local users: Enabled
├── Write access: Disabled
├── Chroot: Disabled
└── Port: 21

Directories:
├── Anonymous: /var/ftp
└── Local users: /home/username

Logs:
├── /var/log/vsftpd.log → vsftpd messages
└── /var/log/xferlog    → Transfer log

Testing:
├── systemctl status vsftpd.service
├── netstat -tupln | grep vsftpd
└── ftp localhost

Process Model:
├── Standalone (default): Always running
└── xinetd: Started on demand
```
