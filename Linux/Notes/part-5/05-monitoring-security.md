# Monitoring Security

## Monitoring Log Files

### Log File Locations

**Primary directory:** `/var/log`

**Common log files:**

| Log File | Location | Description |
|----------|----------|-------------|
| System Messages | `/var/log/messages` | General-purpose log (older systems) |
| Boot Log | `/var/log/boot.log` | Service start/stop messages |
| Kernel Log | `/var/log/dmesg` | Kernel boot messages |
| Security Log | `/var/log/secure` | Login attempts, sudo usage |
| Cron Log | `/var/log/cron` | Cron daemon messages |
| Mail Log | `/var/log/maillog` | Email activity |
| Apache Access | `/var/log/httpd/access_log` | Web server requests |
| Apache Error | `/var/log/httpd/error_log` | Web server errors |
| FTP Log | `/var/log/vsftpd.log` | FTP transfers |
| FTP Transfer | `/var/log/xferlog` | FTP file transfers |
| Samba Log | `/var/log/samba/smbd.log` | Samba file sharing |
| MySQL Log | `/var/log/mysqld.log` | MySQL database |
| YUM Log | `/var/log/yum.log` | Package management |
| X.Org Log | `/var/log/Xorg.0.log` | X server messages |

**Special log files:**

| File | View Command | Description |
|------|--------------|-------------|
| `btmp` | `dump-utmp btmp` | Bad login attempts |
| `wtmp` | `dump-utmp wtmp` | Login/logout history |
| `lastlog` | `lastlog` | Last login per account |
| `dmesg` | `dmesg` | Kernel ring buffer |

---

## Using journalctl

**Modern logging with systemd:**

**View kernel messages:**
```bash
journalctl -k
```

**View service messages:**
```bash
journalctl -u NetworkManager.service
journalctl -u httpd.service
journalctl -u avahi-daemon.service
```

**Follow messages live:**
```bash
# All messages
journalctl -f

# Kernel only
journalctl -k -f

# Specific service
journalctl -f -u NetworkManager.service
```

**View boot messages:**
```bash
# List all boots
journalctl --list-boots

# View specific boot
journalctl -b BOOT_ID
```

**Example:**
```bash
$ journalctl --list-boots
-3 6b968e820df345a781cb6935d483374c Sun 2019-08-25 12:42:08 EDT
-2 f2c5a74fbe9b4cb1ae1c06ac1c24e89b Mon 2019-09-02 15:49:03 EDT
-1 5d26bee1cfb7481a9e4da3dd7f8a80a0 Sun 2019-10-13 12:30:27 EDT
 0 c848e7442932488d91a3a467e8d92fcf Sat 2019-10-19 11:43:04 EDT

$ journalctl -b c848e7442932488d91a3a467e8d92fcf
```

---

## Monitoring User Accounts with auditd

### Setting Up File Watches

**Start watching a file:**
```bash
# Watch /etc/passwd for reads, writes, attribute changes
auditctl -w /etc/passwd -p rwa

# Watch /etc/group
auditctl -w /etc/group -p rwa
```

**Options:**
*   `-w filename` - Watch file (by inode)
*   `-p triggers` - Trigger on access types:
    *   `r` - Read
    *   `w` - Write
    *   `x` - Execute
    *   `a` - Attribute change

**Stop watching:**
```bash
auditctl -W /etc/passwd -p rwa
```

**List current watches:**
```bash
auditctl -l
```

---

### Reviewing Audit Logs

**Search audit logs:**
```bash
ausearch -f /etc/passwd
```

**Example output:**
```
time->Fri Feb  7 04:27:14 2020
type=PATH msg=audit(1328261234.558:574):
  item=0 name="/etc/passwd" inode=170549
  dev=fd:01 mode=0100644 ouid=0 ogid=0
type=SYSCALL msg=audit(1328261234.558:574):
  uid=1000 gid=1000 euid=1000
  comm="vi" exe="/bin/vi"
```

**Key fields:**
*   `time` - When event occurred
*   `name` - File accessed
*   `inode` - File's inode number
*   `uid` - User ID who accessed file
*   `exe` - Program used

**Interpretation:**
*   User ID 1000 (johndoe)
*   Used vi editor
*   Attempted to edit /etc/passwd
*   Suspicious activity - investigate

---

### Audit Daemon Tools

**Key commands:**

| Command | Purpose |
|---------|---------|
| `auditd` | Audit daemon |
| `auditctl` | Control auditing system |
| `ausearch` | Search audit logs |
| `aureport` | Create audit reports |
| `audispd` | Send audit info to other programs |

**Configuration files:**
*   `/etc/audit/auditd.conf` - Daemon configuration
*   `/etc/audit/audit.rules` - Rules loaded at boot

---

## Detecting Bad Passwords

### Using John the Ripper

**Install:**
```bash
dnf install john
apt-get install john
```

**Prepare password file:**
```bash
# Extract passwords
unshadow /etc/passwd /etc/shadow > password.file

# Edit file to remove:
# - Accounts without passwords
# - Accounts you don't want to test
```

**Run password cracker:**
```bash
john password.file
```

**Example output:**
```
Loaded 1 password hash (generic crypt(3) [?/32])
password         (Samantha)
guesses: 1  time: 0:00:00:44  100%  (2)  c/s: 20.87
trying: 12345 - missy
```

**Result:**
*   Cracked "password" in 44 seconds
*   Extremely weak password
*   User needs to change it

**Show cracked passwords:**
```bash
john --show password.file
```

**Warning:**
*   Very CPU-intensive
*   Runs at nice value 19 (low priority)
*   Run on non-production system
*   Run during off-peak hours
*   Test few accounts at a time

**Cleanup:**
```bash
# Remove password file when done
rm password.file
```

**More info:** www.openwall.com/john

---

## Filesystem Scanning

### Verifying Software Packages

**Check package integrity:**
```bash
# Check all packages
rpm -qaV

# Check specific package
rpm -V nmap
```

**Discrepancy codes:**

| Code | Meaning |
|------|---------|
| S | Size changed |
| M | Mode (permissions) changed |
| 5 | MD5 checksum changed |
| D | Device major/minor changed |
| L | Symbolic link changed |
| U | User ownership changed |
| G | Group ownership changed |
| T | Modification time changed |
| P | Capabilities changed |

**Example output:**
```
5S.T.....  c /etc/hba.conf
...T.....    /lib/modules/3.2.1-3.fc16.i686/modules.devname
```

**Ubuntu equivalent:**
```bash
# Install debsums
apt-get install debsums

# Check all packages
debsums -a

# Check one package
debsums packagename
```

---

### Scanning for Modified Binaries

**Find recently modified binaries:**
```bash
# Modified in last 24 hours
find /sbin -mtime -1

# Modified in last 7 days
find /usr/bin -mtime -7
```

**Check file details:**
```bash
stat /sbin/init
```

**Example output:**
```
File: '/sbin/init' -> '../bin/systemd'
Size: 14        Blocks: 0       IO Block: 4096   symbolic link
Access: 2016-02-03 03:34:57.276589176 -0500
Modify: 2016-02-02 23:40:39.139872288 -0500
Change: 2016-02-02 23:40:39.140872415 -0500
```

---

### Additional Filesystem Scans

**Find SUID files:**
```bash
find / -perm -4000
```

**Find SGID files:**
```bash
find / -perm -2000
```

**Find .rhosts files:**
```bash
find /home -name .rhosts
```

**Problem:** Allows system trust without authentication

**Find ownerless files:**
```bash
find / -nouser
```

**Problem:** Not associated with any username

**Find groupless files:**
```bash
find / -nogroup
```

**Problem:** Not associated with any group

---

## Review Questions

1. Where are most log files located?

2. What command views kernel messages from systemd journal?

3. How do you follow live log messages for a specific service?

4. What command starts watching a file with auditd?

5. What tool tests password strength by attempting to crack them?

6. What command verifies RPM package integrity?

7. True or False: John the Ripper should be run on production systems during peak hours.

8. What does the 'S' code mean in rpm -V output?

---

## Quick Reference

```
Log Files:
├── /var/log/messages → General messages (older systems)
├── /var/log/secure   → Security, login, sudo
├── /var/log/boot.log → Boot messages
├── /var/log/cron     → Cron jobs
└── /var/log/dmesg    → Kernel messages

journalctl:
├── journalctl -k              → Kernel messages
├── journalctl -u service      → Service messages
├── journalctl -f              → Follow live
├── journalctl --list-boots    → List boots
└── journalctl -b BOOT_ID      → Specific boot

auditd:
├── auditctl -w file -p rwa    → Watch file
├── auditctl -W file -p rwa    → Stop watching
├── auditctl -l                → List watches
└── ausearch -f file           → Search logs

Audit Triggers:
├── r → Read
├── w → Write
├── x → Execute
└── a → Attribute change

John the Ripper:
├── unshadow /etc/passwd /etc/shadow > file
├── john password.file
├── john --show password.file
└── rm password.file

Package Verification:
├── rpm -qaV           → Check all packages
├── rpm -V package     → Check one package
└── debsums -a         → Ubuntu: check all

rpm -V Codes:
├── S → Size
├── M → Mode (permissions)
├── 5 → MD5 checksum
├── D → Device numbers
├── L → Symbolic link
├── U → User ownership
├── G → Group ownership
├── T → Modification time
└── P → Capabilities

Filesystem Scans:
├── find /sbin -mtime -1       → Modified last 24h
├── find / -perm -4000         → SUID files
├── find / -perm -2000         → SGID files
├── find /home -name .rhosts   → Trust files
├── find / -nouser             → Ownerless
└── find / -nogroup            → Groupless

File Details:
└── stat filename → Show all file metadata

Best Practices:
├── Monitor logs daily
├── Watch critical files with auditd
├── Test passwords regularly
├── Verify package integrity
├── Scan for unauthorized SUID/SGID
└── Check for ownerless files
```
