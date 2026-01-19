# vsftpd Security

## Firewall Configuration

**Required port:**
*   TCP 21 (FTP control)

**Passive mode ports:**
*   TCP 1024-65535 (configurable range)

### Fedora/RHEL 7+ (firewalld)

**Add FTP service:**
```bash
firewall-cmd --permanent --add-service=ftp
firewall-cmd --reload
```

**Verify:**
```bash
firewall-cmd --list-services
```

**Check port:**
```bash
netstat -tupln | grep :21
```

### RHEL 6 (iptables)

**Add to `/etc/sysconfig/iptables`:**
```bash
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
```

**For passive mode, load connection tracking:**
```bash
modprobe nf_conntrack_ftp
```

**Make permanent:**

Add to `/etc/sysconfig/iptables-config`:
```bash
IPTABLES_MODULES="nf_conntrack_ftp"
```

**Restart:**
```bash
service iptables restart
```

---

## SELinux for vsftpd

**Check SELinux mode:**
```bash
getenforce
```

### File Contexts

**View contexts:**
```bash
ls -Z /var/ftp
```

**Correct contexts:**
*   Anonymous FTP: `public_content_t`
*   Upload directory: `public_content_rw_t`
*   User home dirs: `user_home_dir_t`

**Set context for anonymous FTP:**
```bash
chcon -R -t public_content_t /var/ftp/pub
```

**Set context for uploads:**
```bash
chcon -R -t public_content_rw_t /var/ftp/incoming
```

**Make permanent:**
```bash
semanage fcontext -a -t public_content_t "/var/ftp/pub(/.*)?"
semanage fcontext -a -t public_content_rw_t "/var/ftp/incoming(/.*)?"
restorecon -R -v /var/ftp
```

### SELinux Booleans

**View FTP Booleans:**
```bash
getsebool -a | grep ftp
```

**Common Booleans:**

**Allow anonymous FTP:**
```bash
setsebool -P ftpd_anon_write on
```

**Allow home directory access:**
```bash
setsebool -P ftp_home_dir on
```

**Allow full access:**
```bash
setsebool -P ftpd_full_access on
```

**Allow NFS/CIFS:**
```bash
setsebool -P ftpd_use_nfs on
setsebool -P ftpd_use_cifs on
```

**Allow passive mode:**
```bash
setsebool -P ftpd_use_passive_mode on
```

**Read ftpd_selinux man page:**
```bash
man ftpd_selinux
```

---

## Linux File Permissions

**Anonymous FTP directory:**
```bash
ls -ld /var/ftp
drwxr-xr-x. 3 root root 4096 Sep 18 10:00 /var/ftp
```

**Public files (read-only):**
```bash
chmod 755 /var/ftp/pub
chmod 644 /var/ftp/pub/*
```

**Upload directory (write):**
```bash
mkdir /var/ftp/incoming
chmod 730 /var/ftp/incoming
chown ftp:ftp /var/ftp/incoming
```

**⚠️ Security:** Never make /var/ftp writable!

---

## User Access Control

### ftpusers File

**Location:** `/etc/vsftpd/ftpusers`

**Purpose:** Users ALWAYS denied

**Cannot be overridden**

**Example:**
```
root
bin
daemon
```

### user_list File

**Location:** `/etc/vsftpd/user_list`

**Two modes:**

**Deny mode (default):**
```
userlist_enable=YES
userlist_deny=YES
```
*   Users in list are DENIED
*   All others are ALLOWED

**Allow mode:**
```
userlist_enable=YES
userlist_deny=NO
```
*   Only users in list are ALLOWED
*   All others are DENIED

**Example allow list:**
```
chris
jake
mary
```

---

## Password Security

**vsftpd sends passwords in clear text**

**Mitigation:**
*   Use only on trusted networks
*   Consider SFTP instead
*   Enable SSL/TLS (FTPS)

**SSL/TLS configuration:**
```
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
```

---

## Chroot Security

**Purpose:** Restrict users to their home directory

**Enable chroot:**
```
chroot_local_user=YES
```

**Allow write in chroot:**
```
allow_writeable_chroot=YES
```

**⚠️ Security consideration:**
*   Prevents users from accessing system files
*   Users can only see their home directory

---

## Anonymous FTP Security

**Disable anonymous FTP:**
```
anonymous_enable=NO
```

**Enable with restrictions:**
```
anonymous_enable=YES
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO
```

**Allow anonymous uploads (risky):**
```
anon_upload_enable=YES
anon_mkdir_write_enable=YES
```

**⚠️ Warning:** Anonymous uploads can be abused

---

## Connection Limits

**Max clients:**
```
max_clients=50
```

**Max per IP:**
```
max_per_ip=3
```

**Prevents DoS attacks**

---

## Review Questions

1. What port does FTP use?

2. What firewall-cmd command adds FTP service?

3. What SELinux Boolean allows home directory access?

4. What file context is used for anonymous FTP content?

5. True or False: ftpusers can be overridden by user_list.

6. What does chroot_local_user do?

7. How do you disable anonymous FTP?

8. What Boolean allows anonymous FTP writes?

---

## Quick Reference

```
Firewall (firewalld):
├── firewall-cmd --permanent --add-service=ftp
└── firewall-cmd --reload

Firewall (iptables):
└── -A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT

SELinux Contexts:
├── Anonymous FTP: public_content_t
├── Uploads: public_content_rw_t
└── Home dirs: user_home_dir_t

Set Context:
├── chcon -R -t public_content_t /var/ftp/pub
└── restorecon -R -v /var/ftp

SELinux Booleans:
├── setsebool -P ftpd_anon_write on
├── setsebool -P ftp_home_dir on
├── setsebool -P ftpd_full_access on
└── setsebool -P ftpd_use_passive_mode on

File Permissions:
├── /var/ftp: 755 (never writable!)
├── /var/ftp/pub: 755, files 644
└── /var/ftp/incoming: 730, owner ftp:ftp

User Access:
├── /etc/vsftpd/ftpusers → Always denied
└── /etc/vsftpd/user_list → Deny or allow mode

user_list Modes:
├── userlist_deny=YES → Deny listed users
└── userlist_deny=NO  → Allow only listed users

Security Settings:
├── anonymous_enable=NO        → Disable anonymous
├── chroot_local_user=YES      → Restrict to home
├── allow_writeable_chroot=YES → Allow write in chroot
├── max_clients=50             → Limit connections
└── max_per_ip=3               → Limit per IP

SSL/TLS:
├── ssl_enable=YES
├── rsa_cert_file=/etc/ssl/certs/vsftpd.pem
└── rsa_private_key_file=/etc/ssl/private/vsftpd.key
```
