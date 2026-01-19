# Samba Security

## Firewall Configuration

**Required ports:**
*   TCP 445 (SMB)
*   TCP 139 (NetBIOS Session)
*   UDP 137 (NetBIOS Name)
*   UDP 138 (NetBIOS Datagram)

### Fedora/RHEL 7+ (firewalld)

**Add Samba service:**
```bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload
```

**Verify:**
```bash
firewall-cmd --list-services
```

**Check ports:**
```bash
netstat -tupln | grep mbd
```

### RHEL 6 (iptables)

**Add to `/etc/sysconfig/iptables`:**
```bash
-A INPUT -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 137 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 138 -j ACCEPT
```

**Restart:**
```bash
service iptables restart
```

---

## SELinux for Samba

**Check SELinux mode:**
```bash
getenforce
```

### File Contexts

**View contexts:**
```bash
ls -Z /srv/samba
```

**Correct contexts:**
*   Samba shares: `samba_share_t`
*   Public shares: `public_content_t`
*   Home directories: `user_home_dir_t`

**Set context:**
```bash
chcon -R -t samba_share_t /srv/samba/share
```

**Make permanent:**
```bash
semanage fcontext -a -t samba_share_t "/srv/samba/share(/.*)?"
restorecon -R -v /srv/samba/share
```

### SELinux Booleans

**View Samba Booleans:**
```bash
getsebool -a | grep samba
```

**Common Booleans:**

**Allow home directory sharing:**
```bash
setsebool -P samba_enable_home_dirs on
```

**Allow sharing any file:**
```bash
setsebool -P samba_export_all_rw on
```

**Allow domain logons:**
```bash
setsebool -P samba_domain_controller on
```

**Allow NFS/CIFS:**
```bash
setsebool -P samba_share_nfs on
```

**Read samba_selinux man page:**
```bash
man samba_selinux
```

---

## Authentication Methods

**Security modes in smb.conf:**

**User-level (recommended):**
```
security = user
```
*   Each user has account
*   Username/password required
*   Most secure

**Share-level (deprecated):**
```
security = share
```
*   Password per share
*   No usernames
*   Not recommended

**Domain:**
```
security = domain
workgroup = MYDOMAIN
```
*   Authenticate against domain controller
*   Requires domain membership

**ADS (Active Directory):**
```
security = ads
realm = EXAMPLE.COM
workgroup = EXAMPLE
```
*   Active Directory integration
*   Kerberos authentication

---

## User Management

**Samba uses separate password database**

### Adding Samba Users

**1. Create Linux user:**
```bash
useradd -s /sbin/nologin sambauser
```

**2. Set Samba password:**
```bash
smbpasswd -a sambauser
```

**Enter password when prompted**

### Managing Samba Users

**Enable user:**
```bash
smbpasswd -e username
```

**Disable user:**
```bash
smbpasswd -d username
```

**Delete user:**
```bash
smbpasswd -x username
```

**Change password:**
```bash
smbpasswd username
```

**List users:**
```bash
pdbedit -L
```

**Detailed list:**
```bash
pdbedit -Lv
```

---

## File Permissions

**Share directory:**
```bash
mkdir -p /srv/samba/share
chmod 2775 /srv/samba/share
chown root:sambashare /srv/samba/share
```

**SGID bit (2775):**
*   New files inherit group
*   Maintains group ownership

**User files:**
*   Read/write for owner
*   Read/write for group
*   No access for others

---

## Access Control

**Per-share access:**

**Allow specific users:**
```
[share]
    path = /srv/samba/share
    valid users = chris, jake
```

**Deny specific users:**
```
[share]
    path = /srv/samba/share
    invalid users = baduser
```

**Allow groups:**
```
[share]
    path = /srv/samba/share
    valid users = @sambashare
```

**Read-only users:**
```
[share]
    path = /srv/samba/share
    read list = guest
    write list = chris, jake
```

---

## Guest Access

**Enable guest access:**
```
[public]
    path = /srv/samba/public
    guest ok = yes
    browseable = yes
    writable = no
```

**Guest account:**
```
[global]
    guest account = nobody
```

**⚠️ Security:** Use carefully, only for public data

---

## Hosts Allow/Deny

**Allow specific networks:**
```
[global]
    hosts allow = 192.168.1. 127.
```

**Deny specific hosts:**
```
[global]
    hosts deny = 192.168.1.100
```

**Per-share:**
```
[share]
    hosts allow = 192.168.1.0/24
```

---

## Review Questions

1. What ports does Samba use?

2. What firewall-cmd command adds Samba service?

3. What SELinux context is used for Samba shares?

4. What command adds a Samba user?

5. True or False: Samba uses Linux passwords.

6. What does the SGID bit do on directories?

7. How do you allow a group in valid users?

8. What Boolean allows home directory sharing?

---

## Quick Reference

```
Firewall (firewalld):
├── firewall-cmd --permanent --add-service=samba
└── firewall-cmd --reload

Firewall (iptables):
├── -A INPUT -m tcp -p tcp --dport 445 -j ACCEPT
├── -A INPUT -m tcp -p tcp --dport 139 -j ACCEPT
├── -A INPUT -m udp -p udp --dport 137 -j ACCEPT
└── -A INPUT -m udp -p udp --dport 138 -j ACCEPT

SELinux Contexts:
├── Samba shares: samba_share_t
├── Public: public_content_t
└── Home dirs: user_home_dir_t

Set Context:
├── chcon -R -t samba_share_t /path
└── restorecon -R -v /path

SELinux Booleans:
├── setsebool -P samba_enable_home_dirs on
├── setsebool -P samba_export_all_rw on
└── setsebool -P samba_domain_controller on

User Management:
├── useradd -s /sbin/nologin user → Create Linux user
├── smbpasswd -a user              → Add Samba user
├── smbpasswd -e user              → Enable user
├── smbpasswd -d user              → Disable user
├── smbpasswd -x user              → Delete user
└── pdbedit -L                     → List users

File Permissions:
├── mkdir -p /srv/samba/share
├── chmod 2775 /srv/samba/share  → SGID bit
└── chown root:sambashare /srv/samba/share

Security Modes:
├── security = user   → User-level (recommended)
├── security = share  → Share-level (deprecated)
├── security = domain → Domain authentication
└── security = ads    → Active Directory

Access Control:
├── valid users = chris, jake
├── invalid users = baduser
├── valid users = @groupname
├── read list = guest
└── write list = chris

Hosts:
├── hosts allow = 192.168.1. 127.
└── hosts deny = 192.168.1.100

Guest Access:
├── guest ok = yes
└── guest account = nobody
```
