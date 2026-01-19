# vsftpd Configuration

## Configuration File

**Location:** `/etc/vsftpd/vsftpd.conf`

**Format:** `directive=value`

**Comments:** Lines starting with `#`

**Backup before editing:**
```bash
cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
```

**After changes:**
```bash
systemctl restart vsftpd.service
```

---

## Anonymous FTP Configuration

**Enable anonymous FTP:**
```
anonymous_enable=YES
```

**Disable anonymous FTP:**
```
anonymous_enable=NO
```

**Anonymous root directory:**
```
anon_root=/var/ftp
```

**Allow anonymous uploads:**
```
anon_upload_enable=YES
anon_mkdir_write_enable=YES
```

**Permissions for uploads:**
```
anon_umask=022
```

**Example anonymous setup:**
```
anonymous_enable=YES
anon_root=/var/ftp
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO
no_anon_password=YES
```

---

## Local User Configuration

**Enable local users:**
```
local_enable=YES
```

**Allow writes:**
```
write_enable=YES
```

**Local user umask:**
```
local_umask=022
```

**Chroot configuration:**
```
chroot_local_user=YES
allow_writeable_chroot=YES
```

**User home as root:**
*   Users start in their home directory
*   Can navigate entire home (unless chroot enabled)

---

## User Access Control

**Enable user list:**
```
userlist_enable=YES
```

**Deny mode (default):**
```
userlist_deny=YES
```
*   Users in `/etc/vsftpd/user_list` are DENIED

**Allow mode:**
```
userlist_deny=NO
```
*   Only users in `/etc/vsftpd/user_list` are ALLOWED

**User list file:**
```
userlist_file=/etc/vsftpd/user_list
```

---

## Passive Mode Configuration

**Enable passive mode:**
```
pasv_enable=YES
```

**Passive port range:**
```
pasv_min_port=40000
pasv_max_port=40100
```

**Passive address (for NAT):**
```
pasv_address=203.0.113.10
```

**Benefits:**
*   Works through firewalls
*   Required for most Internet FTP

---

## Logging Configuration

**Enable transfer logging:**
```
xferlog_enable=YES
```

**Transfer log location:**
```
xferlog_file=/var/log/xferlog
```

**Standard format:**
```
xferlog_std_format=YES
```

**Enable vsftpd logging:**
```
dual_log_enable=YES
vsftpd_log_file=/var/log/vsftpd.log
```

**Log level:**
```
log_ftp_protocol=YES
```

---

## Connection Settings

**Banner message:**
```
ftpd_banner=Welcome to FTP Server
```

**Idle timeout:**
```
idle_session_timeout=600
```

**Data connection timeout:**
```
data_connection_timeout=120
```

**Max clients:**
```
max_clients=50
```

**Max per IP:**
```
max_per_ip=3
```

**Max login failures:**
```
max_login_fails=3
```

---

## Internet Site Configuration

**For public FTP server:**

**1. Disable anonymous (usually):**
```
anonymous_enable=NO
```

**2. Enable local users:**
```
local_enable=YES
write_enable=YES
```

**3. Chroot users:**
```
chroot_local_user=YES
allow_writeable_chroot=YES
```

**4. Configure passive mode:**
```
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
pasv_address=YOUR_PUBLIC_IP
```

**5. Set connection limits:**
```
max_clients=50
max_per_ip=3
```

**6. Enable logging:**
```
xferlog_enable=YES
dual_log_enable=YES
```

**7. Set banner:**
```
ftpd_banner=Welcome to Example.com FTP
```

---

## Upload Configuration

**Allow uploads:**
```
write_enable=YES
```

**Upload permissions:**
```
local_umask=022
```

**Upload directory ownership:**
```bash
mkdir /home/ftpuser/uploads
chown ftpuser:ftpuser /home/ftpuser/uploads
chmod 755 /home/ftpuser/uploads
```

**Anonymous uploads:**
```
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_umask=022
```

**Upload directory for anonymous:**
```bash
mkdir /var/ftp/incoming
chmod 730 /var/ftp/incoming
chown ftp:ftp /var/ftp/incoming
```

---

## Bandwidth Throttling

**Limit anonymous download speed:**
```
anon_max_rate=50000
```
(50KB/s)

**Limit local user speed:**
```
local_max_rate=100000
```
(100KB/s)

**Values in bytes per second**

---

## Virtual Users

**Enable virtual users:**
```
guest_enable=YES
guest_username=ftp
```

**Virtual user configuration:**
```
user_config_dir=/etc/vsftpd/user_conf
```

**Per-user config files:**
```
/etc/vsftpd/user_conf/username
```

**Example per-user config:**
```
local_root=/var/ftp/users/username
write_enable=YES
```

---

## SSL/TLS Configuration

**Enable SSL:**
```
ssl_enable=YES
```

**Certificate files:**
```
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
```

**Force SSL:**
```
force_local_data_ssl=YES
force_local_logins_ssl=YES
```

**SSL protocols:**
```
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
```

---

## Review Questions

1. Where is the vsftpd configuration file?

2. What directive enables anonymous FTP?

3. What does chroot_local_user do?

4. How do you set passive port range?

5. True or False: userlist_deny=YES means allow mode.

6. What directive limits download speed?

7. How do you enable transfer logging?

8. What port range is recommended for passive mode?

---

## Quick Reference

```
Configuration File:
└── /etc/vsftpd/vsftpd.conf

Anonymous FTP:
├── anonymous_enable=YES
├── anon_root=/var/ftp
├── anon_upload_enable=NO
└── anon_mkdir_write_enable=NO

Local Users:
├── local_enable=YES
├── write_enable=YES
├── local_umask=022
├── chroot_local_user=YES
└── allow_writeable_chroot=YES

User Access:
├── userlist_enable=YES
├── userlist_deny=YES  (deny mode)
└── userlist_deny=NO   (allow mode)

Passive Mode:
├── pasv_enable=YES
├── pasv_min_port=40000
├── pasv_max_port=40100
└── pasv_address=PUBLIC_IP

Logging:
├── xferlog_enable=YES
├── xferlog_file=/var/log/xferlog
├── dual_log_enable=YES
└── vsftpd_log_file=/var/log/vsftpd.log

Connection Limits:
├── max_clients=50
├── max_per_ip=3
├── idle_session_timeout=600
└── max_login_fails=3

Bandwidth:
├── anon_max_rate=50000   (bytes/sec)
└── local_max_rate=100000 (bytes/sec)

SSL/TLS:
├── ssl_enable=YES
├── rsa_cert_file=/etc/ssl/certs/vsftpd.pem
├── rsa_private_key_file=/etc/ssl/private/vsftpd.key
├── force_local_data_ssl=YES
└── force_local_logins_ssl=YES

Apply Changes:
└── systemctl restart vsftpd.service
```
