# System Monitoring

## System Activity Reporter (sar)

**Package:** `sysstat`

**Purpose:** Monitor system performance over time

**Installation:**
```bash
yum install sysstat
systemctl enable sysstat
systemctl start sysstat
```

### How sar Works

**Data collection:**
*   `sadc` runs every 10 minutes (from cron)
*   Gathers CPU, memory, disk, network data
*   Stores in `/var/log/sa/sa##` files

**Data viewing:**
*   `sar` command reads stored data
*   Can display historical or live data

---

## sar Commands

### CPU Usage

```bash
sar -u | less
```

**Output:**
```
11:30:05 PM CPU  %user %nice %system %iowait %steal %idle
11:40:06 PM all   0.90  0.00   1.81    1.44   0.28  95.57
```

**Fields:**
*   `%user` - User processes
*   `%system` - Kernel processes
*   `%iowait` - Waiting for I/O
*   `%idle` - Idle time

### Disk Activity

```bash
sar -d | less
```

**Shows:**
*   Transfers per second (tps)
*   KB read/written
*   Average request size
*   Average wait time

### Network Activity

```bash
sar -n DEV | less
```

**Shows:**
*   Packets received/transmitted
*   KB received/transmitted
*   Per network interface

---

## Live sar Monitoring

**Syntax:** `sar [options] interval count`

**Example:**
```bash
# Network stats every 5 seconds, 2 times
sar -n DEV 5 2
```

**Output:**
```
11:19:36 PM IFACE rxpck/s txpck/s rxkB/s txkB/s
11:19:41 PM lo      5.42    5.42   1.06   1.06
11:19:41 PM ens3    0.00    0.00   0.00   0.00
Average:    lo      7.21    7.21   1.42   1.42
Average:    ens3    0.00    0.00   0.00   0.00
```

---

## Cockpit - Web-Based Monitoring

**Access:** `https://localhost:9090`

**Features:**
*   Real-time CPU usage graph
*   Memory and swap consumption
*   Disk I/O activity
*   Network traffic
*   Service management
*   Log viewing

**Installation:**
```bash
yum install cockpit
systemctl enable --now cockpit.socket
firewall-cmd --permanent --add-service=cockpit
firewall-cmd --reload
```

---

## Disk Space Monitoring

### df - Filesystem Space

**Show all filesystems:**
```bash
df
```

**Human-readable format:**
```bash
df -h
```

**Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        29G  2.9G   24G  11% /
/dev/sda2        46M  8.2M   25M  19% /boot
```

**Useful options:**
*   `-h` - Human-readable (GB, MB)
*   `-t type` - Show only specific filesystem type
*   `-x type` - Exclude specific filesystem type
*   `-i` - Show inode usage

**Example - exclude tmpfs:**
```bash
df -h -x tmpfs -x devtmpfs
```

---

### du - Directory Space Usage

**Check directory size:**
```bash
du /home/jake
```

**Human-readable:**
```bash
du -h /home/jake
```

**Output:**
```
114k  /home/jake/httpd/stuff
234k  /home/jake/httpd
137k  /home/jake/uucp/data
701k  /home/jake/uucp
1.0M  /home/jake
```

**Summary only:**
```bash
du -sh /home/jake
```

**Output:**
```
1.0M  /home/jake
```

---

### find - Find Large Files

**Find files by user:**
```bash
find / -xdev -user jake -print | xargs ls -ldS > /tmp/jake
```

**Find files over 100MB:**
```bash
find / -xdev -size +100M | xargs ls -ldS > /tmp/size
```

**Options:**
*   `-xdev` - Don't cross filesystems
*   `-user name` - Files owned by user
*   `-size +100M` - Files larger than 100MB

**View results:**
```bash
less /tmp/size
```

---

## Monitoring Best Practices

**1. Watch disk space regularly**
*   Run `df -h` daily
*   Set up alerts for >80% usage

**2. Review logs**
*   Check logwatch emails
*   Look for repeated failures
*   Watch for authentication attempts

**3. Monitor performance trends**
*   Use sar to identify patterns
*   Look for CPU/memory spikes
*   Check disk I/O bottlenecks

**4. Keep software updated**
*   Apply security patches
*   Use `yum update` / `dnf update`

**5. Check for intrusions**
*   Run `rpm -V` to verify packages
*   Look for modified system files

---

## Review Questions

1. What package provides the sar command?

2. How often does sar collect data by default?

3. What command shows CPU usage with sar?

4. What port does Cockpit use?

5. How do you show disk space in human-readable format?

6. True or False: du shows space used by files and directories.

7. What option prevents find from crossing filesystems?

8. Where does sar store collected data?

---

## Quick Reference

```
sar (System Activity Reporter):
├── yum install sysstat       → Install
├── systemctl enable sysstat  → Enable
├── sar -u                    → CPU usage
├── sar -d                    → Disk activity
├── sar -n DEV                → Network activity
└── sar -n DEV 5 2            → Live (5s, 2 times)

Cockpit:
├── https://localhost:9090    → Access
├── yum install cockpit       → Install
└── systemctl enable --now cockpit.socket → Start

Disk Space:
├── df -h                     → All filesystems
├── df -h -x tmpfs            → Exclude tmpfs
├── du -h /path               → Directory size
├── du -sh /path              → Summary only
└── df -i                     → Inode usage

Find Large Files:
├── find / -xdev -size +100M  → Files >100MB
├── find / -xdev -user name   → User's files
└── xargs ls -ldS             → Sort by size

Data Locations:
├── /var/log/sa/sa##          → sar data files
├── /var/log/messages         → System messages
└── /var/log/secure           → Auth logs

Monitoring Schedule:
├── sadc runs every 10 min    → Collect data
├── logwatch runs daily       → Email report
└── logrotate runs daily      → Rotate logs
```
