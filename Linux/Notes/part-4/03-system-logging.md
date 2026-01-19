# System Logging with rsyslog

## rsyslog Overview

**Purpose:** Centralized logging for Linux systems

**Features:**
*   Gathers log messages from services/applications
*   Directs messages to appropriate log files
*   Supports remote logging
*   Works with systemd journal

---

## rsyslog Components

**Daemon:** `rsyslogd`

**Config file:** `/etc/rsyslog.conf`

**Log directory:** `/var/log/`

**Integration:** Reads from systemd journal

---

## /etc/rsyslog.conf Structure

### Modules Section

```bash
# Local system logging
module(load="imuxsock")

# systemd journal access
module(load="imjournal" StateFile="imjournal.state")

# Kernel messages (from journal)
#module(load="imklog")

# UDP remote logging (disabled by default)
#module(load="imudp")
#input(type="imudp" port="514")

# TCP remote logging (disabled by default)
#module(load="imtcp")
#input(type="imtcp" port="514")
```

**Key modules:**
*   `imjournal` - Read systemd journal
*   `imuxsock` - Local system messages
*   `imudp` - Remote logging (UDP port 514)
*   `imtcp` - Remote logging (TCP port 514)

---

## Rules Section

**Format:** `facility.priority  destination`

### Example Rules

```bash
# All messages (except mail/auth/cron) → /var/log/messages
*.info;mail.none;authpriv.none;cron.none  /var/log/messages

# Authentication messages → /var/log/secure
authpriv.*  /var/log/secure

# Mail messages → /var/log/maillog
mail.*  -/var/log/maillog

# Cron messages → /var/log/cron
cron.*  /var/log/cron

# Kernel messages → console (commented out)
#kern.*  /dev/console
```

### Facilities

Common facilities:
*   `kern` - Kernel messages
*   `mail` - Mail system
*   `authpriv` - Authentication/security
*   `cron` - Cron daemon
*   `*` - All facilities

### Priorities (lowest to highest)

*   `debug` - Debug messages
*   `info` - Informational
*   `notice` - Normal but significant
*   `warning` - Warning conditions
*   `err` - Error conditions
*   `crit` - Critical conditions
*   `alert` - Action must be taken
*   `emerg` - System unusable

**Priority matching:** Specified level and higher

Example: `mail.info` matches info, notice, warning, err, crit, alert, emerg

---

## Log Message Format

**Default format:** `RSYSLOG_TraditionalFileFormat`

**Example from /var/log/messages:**
```
Feb 25 11:04:32 toys network: Bringing up loopback: succeeded
Feb 25 13:01:14 toys vsftpd(pam_unix)[10565]: authentication failure
```

**Fields (left to right):**
1. Date/time
2. Hostname
3. Program/service name
4. Process ID [in brackets]
5. Message text

---

## Remote Logging (Loghost)

### Client Configuration

**Send logs to remote loghost:**

```bash
# Edit /etc/rsyslog.conf
# Add @ before hostname to send remotely

*.info;mail.none;authpriv.none;cron.none  @loghost
authpriv.*  @loghost
mail.*  @loghost
```

**Restart rsyslog:**
```bash
systemctl restart rsyslog
```

### Server (Loghost) Configuration

**1. Enable remote logging:**

```bash
# Edit /etc/rsyslog.conf
# Uncomment these lines:

# UDP (port 514)
module(load="imudp")
input(type="imudp" port="514")

# TCP (port 514)
module(load="imtcp")
input(type="imtcp" port="514")
```

**2. Open firewall:**
```bash
firewall-cmd --permanent --add-port=514/udp
firewall-cmd --permanent --add-port=514/tcp
firewall-cmd --reload
```

**3. Restart rsyslog:**
```bash
systemctl restart rsyslog
```

**4. Verify listening:**
```bash
netstat -tupln | grep 514
```

**Output:**
```
tcp  0  0 0.0.0.0:514  0.0.0.0:*  LISTEN  25341/rsyslogd
udp  0  0 0.0.0.0:514  0.0.0.0:*         25341/rsyslogd
```

---

## logwatch - Daily Log Reports

**Purpose:** Email daily log summaries

**Installation:**
```bash
yum install logwatch
```

**How it works:**
*   Runs from cron (`/etc/cron.daily/0logwatch`)
*   Scans log files for problems
*   Emails report to administrator

**Configuration:**
*   Default: `/usr/share/logwatch/default.conf/logwatch.conf`
*   Local: `/etc/logwatch/conf/logwatch.conf`

**Change email recipient:**
```bash
# Edit /etc/logwatch/conf/logwatch.conf
MailTo = admin@example.com
```

**View logwatch emails:**
```bash
mail
```

**Report includes:**
*   Kernel errors
*   Authentication failures
*   Disk space usage
*   Installed packages
*   Service failures

---

## logrotate - Log File Management

**Purpose:** Rotate and compress old logs

**Config:** `/etc/logrotate.conf`

**How it works:**
*   Runs daily from cron
*   Backs up log files
*   Compresses old logs
*   Deletes very old logs
*   Prevents disk space issues

---

## Viewing systemd Journal

**Command:** `journalctl`

**Examples:**
```bash
# View all logs
journalctl

# Follow new messages
journalctl -f

# Show only kernel messages
journalctl -k

# Show logs for specific service
journalctl -u sshd

# Show logs since boot
journalctl -b

# Show logs for time range
journalctl --since "2024-01-01" --until "2024-01-02"
```

---

## Review Questions

1. What daemon provides system logging?

2. Where is the rsyslog configuration file?

3. What does `mail.info` mean in rsyslog rules?

4. How do you send logs to a remote loghost?

5. What port does rsyslog use for remote logging?

6. True or False: logwatch runs every hour.

7. What command views the systemd journal?

8. Where are most log files stored?

---

## Quick Reference

```
rsyslog:
├── /etc/rsyslog.conf        → Main config
├── /var/log/                → Log directory
├── systemctl restart rsyslog → Restart service
└── journalctl               → View systemd journal

Log Files:
├── /var/log/messages        → General messages
├── /var/log/secure          → Authentication
├── /var/log/maillog         → Mail
└── /var/log/cron            → Cron jobs

Rule Format:
facility.priority  destination

Examples:
├── mail.info  /var/log/maillog     → Local file
├── *.crit  @loghost                → Remote (UDP)
└── kern.*  /dev/console            → Console

Remote Logging:
Client:
└── Add @loghost to rules

Server:
├── Enable imudp/imtcp modules
├── Open firewall port 514
└── Restart rsyslog

logwatch:
├── yum install logwatch     → Install
├── /etc/logwatch/conf/      → Config
└── MailTo = email@addr      → Set recipient

journalctl:
├── journalctl               → View all
├── journalctl -f            → Follow
├── journalctl -u service    → Specific service
└── journalctl -b            → Since boot
```
