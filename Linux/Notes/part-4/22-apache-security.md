# Apache Security

## File Permissions and Ownership

**Apache user and group:**
*   User: `apache`
*   Group: `apache`
*   Shell: `/sbin/nologin` (cannot log in)

**DocumentRoot permissions:**
```bash
ls -ld /var/www/html
drwxr-xr-x. 2 root root 4096 Sep 18 10:00 /var/www/html
```

**File permissions:**
*   Files: `644` (rw-r--r--)
*   Directories: `755` (rwxr-xr-x)
*   Execute bit on directories required for traversal

**Setting permissions:**
```bash
chmod 644 /var/www/html/*.html
chmod 755 /var/www/html/subdir
```

**Ownership:**
```bash
chown apache:apache /var/www/html/file.html
```

---

## Firewall Configuration

**Required ports:**
*   TCP 80 (HTTP)
*   TCP 443 (HTTPS)

### Fedora/RHEL 7+ (firewalld)

**Check status:**
```bash
firewall-cmd --list-services
```

**Add HTTP:**
```bash
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
```

**Add HTTPS:**
```bash
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
```

**Or add both:**
```bash
firewall-cmd --permanent --add-service={http,https}
firewall-cmd --reload
```

**Verify:**
```bash
netstat -tupln | grep httpd
```

### RHEL 6 (iptables)

**Add to `/etc/sysconfig/iptables`:**
```bash
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```

**Restart:**
```bash
service iptables restart
```

---

## SELinux for Apache

**Check SELinux mode:**
```bash
getenforce
grep ^SELINUX= /etc/sysconfig/selinux
```

### File Contexts

**View contexts:**
```bash
ls -Z /var/www/html
```

**Correct contexts:**
*   Content: `httpd_sys_content_t`
*   Config: `httpd_config_t`
*   Logs: `httpd_log_t`
*   Scripts: `httpd_sys_script_exec_t`

**Set context:**
```bash
chcon -R -t httpd_sys_content_t /var/www/html
```

**Restore context:**
```bash
restorecon -R -v /var/www/html
```

**Make permanent:**
```bash
semanage fcontext -a -t httpd_sys_content_t "/custom/path(/.*)?"
restorecon -R -v /custom/path
```

### SELinux Booleans

**View all httpd Booleans:**
```bash
getsebool -a | grep httpd
```

**Common Booleans:**

**Allow home directories:**
```bash
setsebool -P httpd_enable_homedirs on
```

**Allow network connections:**
```bash
setsebool -P httpd_can_network_connect on
```

**Allow database connections:**
```bash
setsebool -P httpd_can_network_connect_db on
```

**Allow email sending:**
```bash
setsebool -P httpd_can_sendmail on
```

**Allow NFS/CIFS:**
```bash
setsebool -P httpd_use_nfs on
setsebool -P httpd_use_cifs on
```

**Read httpd_selinux man page:**
```bash
man httpd_selinux
```

(Install `selinux-policy-doc` if not available)

---

## Access Control

**Allow from specific IP:**
```apache
<Directory /var/www/html/private>
    Require ip 192.168.1.0/24
</Directory>
```

**Deny from specific IP:**
```apache
<Directory /var/www/html>
    <RequireAll>
        Require all granted
        Require not ip 10.0.0.0/8
    </RequireAll>
</Directory>
```

**Allow specific host:**
```apache
<Directory /var/www/html/admin>
    Require host example.com
</Directory>
```

---

## Basic Authentication

**Create password file:**
```bash
htpasswd -c /etc/httpd/.htpasswd username
```

**Add more users:**
```bash
htpasswd /etc/httpd/.htpasswd another_user
```

**Configure directory:**
```apache
<Directory /var/www/html/secure>
    AuthType Basic
    AuthName "Restricted Area"
    AuthUserFile /etc/httpd/.htpasswd
    Require valid-user
</Directory>
```

**Or in .htaccess:**
```apache
AuthType Basic
AuthName "Members Only"
AuthUserFile /etc/httpd/.htpasswd
Require valid-user
```

---

## Disable Directory Listings

**In httpd.conf:**
```apache
<Directory /var/www/html>
    Options -Indexes
</Directory>
```

**Or remove Indexes:**
```apache
<Directory /var/www/html>
    Options FollowSymLinks
</Directory>
```

---

## Disable .htaccess

**For performance and security:**
```apache
<Directory /var/www/html>
    AllowOverride None
</Directory>
```

---

## Hide Apache Version

**In httpd.conf:**
```apache
ServerTokens Prod
ServerSignature Off
```

**ServerTokens options:**
*   `Prod` - Apache only
*   `Major` - Apache/2
*   `Minor` - Apache/2.4
*   `Min` - Apache/2.4.41
*   `OS` - Apache/2.4.41 (Unix)
*   `Full` - Apache/2.4.41 (Unix) PHP/7.3.8

---

## Limit Request Size

**Prevent large uploads:**
```apache
LimitRequestBody 10485760
```

(10MB in bytes)

---

## Review Questions

1. What user does Apache run as by default?

2. What ports must be open for HTTP and HTTPS?

3. What SELinux file context is used for web content?

4. How do you enable httpd home directories in SELinux?

5. What command creates an Apache password file?

6. True or False: .htaccess files improve performance.

7. What directive hides the Apache version?

8. How do you allow access only from 192.168.1.0/24?

---

## Quick Reference

```
File Permissions:
├── Files: 644 (rw-r--r--)
├── Directories: 755 (rwxr-xr-x)
└── Owner: apache:apache

Firewall (firewalld):
├── firewall-cmd --permanent --add-service=http
├── firewall-cmd --permanent --add-service=https
└── firewall-cmd --reload

Firewall (iptables):
└── -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

SELinux Contexts:
├── Content: httpd_sys_content_t
├── Config: httpd_config_t
└── Logs: httpd_log_t

Set Context:
├── chcon -R -t httpd_sys_content_t /path
└── restorecon -R -v /path

SELinux Booleans:
├── setsebool -P httpd_enable_homedirs on
├── setsebool -P httpd_can_network_connect on
└── setsebool -P httpd_can_network_connect_db on

Access Control:
├── Require all granted
├── Require ip 192.168.1.0/24
├── Require host example.com
└── Require valid-user

Basic Auth:
├── htpasswd -c /etc/httpd/.htpasswd user
├── AuthType Basic
├── AuthUserFile /etc/httpd/.htpasswd
└── Require valid-user

Security Settings:
├── Options -Indexes          → Disable listings
├── AllowOverride None        → Disable .htaccess
├── ServerTokens Prod         → Hide version
├── ServerSignature Off       → Hide signature
└── LimitRequestBody 10485760 → Limit uploads
```
