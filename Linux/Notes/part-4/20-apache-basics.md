# Apache Basics

## Apache HTTP Server Overview

**What is Apache?**
*   Most popular web server worldwide
*   Open source project (Apache Software Foundation)
*   Also called Apache HTTPD or httpd
*   Serves web content over HTTP/HTTPS

**History:**
*   Started as patches to NCSA HTTPD (1995)
*   "A patchy server" → Apache
*   Apache Group → Apache Software Foundation (ASF)
*   ASF now has 350+ open source projects

**Key features:**
*   Modular architecture
*   Virtual hosting
*   SSL/TLS support
*   Multiple authentication methods
*   Scripting language support (PHP, Perl, Python)

---

## httpd Package

**Package name:**
*   Fedora/RHEL: `httpd`
*   Ubuntu/Debian: `apache2`

**Main daemon:**
*   `/usr/sbin/httpd` (RHEL/Fedora)
*   `/usr/sbin/apache2` (Ubuntu/Debian)

**Check package info:**
```bash
rpm -qi httpd
```

**Package contents:**
```bash
rpm -ql httpd          # List all files
rpm -qc httpd          # Configuration files
rpm -qd httpd          # Documentation
```

---

## Installation

### Fedora/RHEL

**Install httpd:**
```bash
yum install httpd
```

**Install Web Server group:**
```bash
yum groupinstall "Web Server"
```

**Group includes:**
*   `httpd` - Main server
*   `httpd-manual` - Documentation
*   `mod_ssl` - SSL/TLS support
*   `crypto-utils` - Certificate generation
*   `mod_perl` - Perl support
*   `php` - PHP support
*   `php-ldap` - LDAP support for PHP
*   `php-mysql` - MySQL support for PHP

### Ubuntu/Debian

**Install Apache:**
```bash
apt-get install apache2
```

---

## Key Directories and Files

### Configuration Files (RHEL/Fedora)

**Main config:**
*   `/etc/httpd/conf/httpd.conf`

**Module configs:**
*   `/etc/httpd/conf.d/*.conf`
*   `/etc/httpd/conf.modules.d/*.conf`

**Example module configs:**
*   `/etc/httpd/conf.d/ssl.conf` - SSL configuration
*   `/etc/httpd/conf.d/php.conf` - PHP configuration
*   `/etc/httpd/conf.d/welcome.conf` - Default page

### Content Directories

**DocumentRoot (default):**
*   `/var/www/html` - Web content

**CGI scripts:**
*   `/var/www/cgi-bin` - CGI scripts

**Manual:**
*   `/var/www/manual` - Apache documentation

### Log Files

**Access log:**
*   `/var/log/httpd/access_log`

**Error log:**
*   `/var/log/httpd/error_log`

**SSL logs:**
*   `/var/log/httpd/ssl_access_log`
*   `/var/log/httpd/ssl_error_log`

---

## Starting Apache

### RHEL 6 and Earlier

**Enable at boot:**
```bash
chkconfig httpd on
```

**Start service:**
```bash
service httpd start
```

**Check status:**
```bash
service httpd status
```

### RHEL 7/8 and Fedora

**Enable at boot:**
```bash
systemctl enable httpd.service
```

**Start service:**
```bash
systemctl start httpd.service
```

**Check status:**
```bash
systemctl status httpd.service
```

**Example output:**
```
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled)
   Active: active (running) since Mon 2019-09-02 16:16:56 EDT
   Main PID: 11773 (/usr/sbin/httpd)
   Status: "Total requests: 14; Idle/Busy workers 100/0"
```

**Restart:**
```bash
systemctl restart httpd.service
```

**Reload config:**
```bash
systemctl reload httpd.service
```

---

## Testing Apache

**Check if running:**
```bash
systemctl status httpd.service
```

**Check listening ports:**
```bash
netstat -tupln | grep httpd
```

**Expected output:**
```
tcp6  0  0  :::80   :::*  LISTEN  11773/httpd
tcp6  0  0  :::443  :::*  LISTEN  11773/httpd
```

**Test from browser:**
```
http://localhost
```

**Test from command line:**
```bash
curl http://localhost
```

---

## Apache Processes

**Default process model:**
*   Parent process (runs as root)
*   5-6 child processes (run as apache user)

**View processes:**
```bash
ps -ef | grep httpd
```

**Example output:**
```
root     11773  1     0 16:16 ?  00:00:01 /usr/sbin/httpd -DFOREGROUND
apache   11774  11773 0 16:16 ?  00:00:00 /usr/sbin/httpd -DFOREGROUND
apache   11775  11773 0 16:16 ?  00:00:00 /usr/sbin/httpd -DFOREGROUND
```

---

## httpd-manual Package

**Purpose:** Apache documentation

**Installation:**
```bash
yum install httpd-manual
```

**Access:**
```
http://localhost/manual
```

**Contents:**
*   Complete Apache documentation
*   Configuration directives
*   Module documentation
*   How-to guides

---

## Default Ports

**HTTP:**
*   Port 80 (TCP)
*   Standard web traffic

**HTTPS:**
*   Port 443 (TCP)
*   Encrypted web traffic
*   Requires mod_ssl

**Check ports:**
```bash
netstat -tupln | grep httpd
```

---

## Review Questions

1. What is the main Apache package name in RHEL/Fedora?

2. What is the default DocumentRoot directory?

3. What port does HTTP use by default?

4. Where is the main Apache configuration file located?

5. What command starts Apache in RHEL 7+?

6. True or False: Apache child processes run as root.

7. How do you access Apache documentation after installing httpd-manual?

8. What are the two main log files for Apache?

---

## Quick Reference

```
Package:
└── yum install httpd

Configuration:
├── /etc/httpd/conf/httpd.conf       → Main config
├── /etc/httpd/conf.d/*.conf         → Module configs
└── /etc/httpd/conf.modules.d/*.conf → Module loading

Content:
├── /var/www/html     → DocumentRoot
├── /var/www/cgi-bin  → CGI scripts
└── /var/www/manual   → Documentation

Logs:
├── /var/log/httpd/access_log → Access log
└── /var/log/httpd/error_log  → Error log

Service (RHEL 7+):
├── systemctl enable httpd.service  → Enable at boot
├── systemctl start httpd.service   → Start
├── systemctl status httpd.service  → Check status
├── systemctl restart httpd.service → Restart
└── systemctl reload httpd.service  → Reload config

Service (RHEL 6):
├── chkconfig httpd on  → Enable at boot
├── service httpd start → Start
└── service httpd status → Check status

Ports:
├── 80  → HTTP
└── 443 → HTTPS (requires mod_ssl)

Testing:
├── http://localhost
├── curl http://localhost
└── netstat -tupln | grep httpd

Processes:
├── Parent: runs as root
└── Children: run as apache user
```
