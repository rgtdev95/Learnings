# Server Administration Basics

## The 5-Step Server Setup Process

Every Linux server follows the same basic setup pattern:

```
1. Install → 2. Configure → 3. Start → 4. Secure → 5. Monitor
```

---

## Step 1: Install the Server

**Package Installation:**
*   Most server software NOT preinstalled by default
*   Available through package manager (yum/dnf/apt)
*   Often organized in package groups/collections

### Common Server Package Categories

| Server Type | Package(s) | Purpose |
|-------------|-----------|---------|
| **System Logging** | `rsyslog` | Local/remote log gathering |
| **Print Server** | `cups` | CUPS printing service |
| **Web Server** | `httpd`, `apache2` | Apache web server |
| **FTP Server** | `vsftpd` | FTP file transfer |
| **File Server (Windows)** | `samba` | Windows file/print sharing |
| **File Server (Linux)** | `nfs-utils` | NFS file sharing |
| **Mail Server** | `postfix`, `sendmail` | Email (MTA) |
| **Directory Server** | `openldap-servers`, `krb5-server` | LDAP, Kerberos auth |
| **DNS Server** | `bind` | Domain name resolution |
| **Time Server** | `chrony`, `ntpd` | Network Time Protocol |
| **Database** | `postgresql`, `mysql`, `mariadb` | SQL databases |

---

## Step 2: Configure the Server

### Configuration Files

**Location:** `/etc/` directory (or subdirectories)

**Format:** Plain-text files

**Examples:**
*   `/etc/httpd/conf/httpd.conf` - Apache main config
*   `/etc/httpd/conf.d/*.conf` - Apache additional configs
*   `/etc/ssh/sshd_config` - SSH server config

### Configuration Best Practices

✅ **Use vim for editing:**
*   Syntax highlighting catches errors
*   Color changes if you make mistakes

✅ **Check default configuration:**
*   Most packages install with minimal/secure defaults
*   Services often listen only on localhost initially
*   Must configure to make fully functional

✅ **Test before deploying:**
*   Some services have test commands
*   Start service to check for errors

**Example - Mail/DNS servers:**
*   Install with defaults
*   Listen only on localhost
*   Must configure to accept remote connections

---

## Step 3: Start the Server

### Service Management

**Modern systems (RHEL 7+, Fedora, Ubuntu):** `systemd`

**Older systems (RHEL 6):** SysVinit scripts

### Common Service Commands

```bash
# Check status
systemctl status servicename

# Start service
systemctl start servicename

# Stop service
systemctl stop servicename

# Restart service
systemctl restart servicename

# Enable at boot
systemctl enable servicename

# Disable at boot
systemctl disable servicename
```

### Daemon Processes

**Characteristics:**
*   Run continuously in background
*   Listen for requests
*   Often run as non-root user

**User/Group Permissions:**
*   `httpd` runs as `apache` user
*   `ntpd` runs as `ntp` user
*   **Why?** Limits damage if service is compromised

**Daemon Configuration:**
*   `/etc/sysconfig/servicename` - Daemon options
*   Options passed to daemon at startup
*   Example: `/etc/sysconfig/rsyslogd`

**Port Numbers:**
*   Each service listens on specific port(s)
*   Standard ports (e.g., SSH=22, HTTP=80, HTTPS=443)
*   Must open firewall for service ports
*   SELinux may restrict non-standard ports

---

## Step 4: Secure the Server

### Security Layers

**1. Password Protection**

*   Strong passwords required
*   Disable direct root login
*   Use `su` or `sudo` for privilege escalation
*   Consider public key authentication

**2. Firewalls**

*   `iptables` - Packet filtering
*   `firewalld` - Dynamic firewall management
*   Open only necessary ports
*   Block/allow specific IP addresses

**3. TCP Wrappers (Legacy)**

*   `/etc/hosts.allow` - Allow access
*   `/etc/hosts.deny` - Deny access
*   Less common now, but still checked by some daemons

**4. SELinux**

*   Protects system from compromised services
*   Restricts file access by context
*   Controls service features with Booleans
*   Limits port access
*   Default: Enforcing mode

**5. Service Configuration**

*   Restrict access by username
*   Restrict access by hostname/IP
*   Limit features available to users

---

## Step 5: Monitor the Server

### Monitoring Tools

**1. System Logging (`rsyslog`)**
*   Gather log messages
*   Store in `/var/log/`
*   Can forward to central loghost

**2. Log Analysis (`logwatch`)**
*   Scans logs nightly
*   Emails critical information
*   Highlights potential problems

**3. Log Rotation (`logrotate`)**
*   Backs up log files
*   Compresses old logs
*   Prevents disk space issues

**4. System Activity Reports (`sar`)**
*   Monitor CPU, memory, disk, network
*   Gather data continuously
*   Review historical performance

**5. Cockpit Web UI**
*   Real-time monitoring
*   CPU, memory, disk I/O, network
*   Web-based interface (port 9090)

**6. Keep Software Updated**
*   Install security patches
*   Use `yum update` / `dnf update`
*   Subscribe to security advisories

**7. Filesystem Integrity**
*   Check for tampering
*   Use `rpm -V` to verify packages
*   Look for unauthorized changes

---

## Server vs Desktop Differences

| Aspect | Desktop | Server |
|--------|---------|--------|
| **Access** | Local console | Remote (SSH) |
| **Uptime** | Turn off when not in use | 24/7/365 |
| **Security** | Can close all ports | Must allow service access |
| **Monitoring** | Manual | Automated logging/alerts |
| **Location** | On desk | Rack in data center |

---

## Security by Obscurity

**Example:** Change SSH from port 22 to high port number

**Pros:**
*   Reduces automated attacks
*   Port scanners may miss it

**Cons:**
*   Not real security
*   Inconvenient for legitimate users
*   Must tell users the port number

---

## Review Questions

1. What are the 5 steps of server setup?

2. Where are most server configuration files located?

3. What command starts a service in systemd?

4. Why do daemon processes run as non-root users?

5. True or False: Most server packages are fully functional immediately after installation.

6. What is the default port for SSH?

7. Name three security layers for Linux servers.

8. What tool provides web-based server monitoring?

---

## Quick Reference

```
5-Step Server Setup:
1. Install    → yum/dnf/apt install package
2. Configure  → Edit /etc/config files
3. Start      → systemctl start/enable service
4. Secure     → Firewall, SELinux, passwords
5. Monitor    → rsyslog, sar, Cockpit

Service Management:
├── systemctl status service   → Check status
├── systemctl start service    → Start now
├── systemctl enable service   → Start at boot
├── systemctl restart service  → Restart
└── systemctl stop service     → Stop

Security Layers:
├── Passwords     → Strong, no direct root
├── Firewall      → iptables, firewalld
├── SELinux       → File contexts, Booleans
└── Config files  → Restrict by user/IP

Monitoring:
├── rsyslog    → System logging
├── logwatch   → Daily email reports
├── sar        → Performance data
├── Cockpit    → Web UI (port 9090)
└── df/du      → Disk space

Common Ports:
├── 22  → SSH
├── 80  → HTTP
├── 443 → HTTPS
├── 25  → SMTP (mail)
└── 53  → DNS
```
