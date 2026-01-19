# Samba Installation

## Installing Samba Packages

### Fedora/RHEL 8

**Install Samba:**
```bash
yum install samba samba-client samba-common
```

**Optional packages:**
```bash
yum install samba-winbind samba-winbind-clients
```

**Check installation:**
```bash
rpm -qa | grep samba
```

**Example output:**
```
samba-4.10.4-101.el8_1.x86_64
samba-client-4.10.4-101.el8_1.x86_64
samba-common-4.10.4-101.el8_1.noarch
samba-common-tools-4.10.4-101.el8_1.x86_64
samba-libs-4.10.4-101.el8_1.x86_64
samba-winbind-4.10.4-101.el8_1.x86_64
```

### Ubuntu/Debian

**Install Samba:**
```bash
apt-get install samba samba-common-bin
```

---

## Package Contents

**Main packages:**

**samba:**
*   smbd daemon
*   nmbd daemon
*   Configuration files

**samba-client:**
*   smbclient command
*   Client utilities

**samba-common:**
*   Common files
*   Configuration templates

**samba-winbind:**
*   winbindd daemon
*   Domain integration

---

## Configuration Files

**Main config:**
*   `/etc/samba/smb.conf`

**User database:**
*   `/var/lib/samba/private/passdb.tdb`

**Other files:**
*   `/etc/samba/lmhosts` - NetBIOS name mapping
*   `/etc/sysconfig/samba` - Daemon options

**Check config files:**
```bash
rpm -qc samba-common
```

**Output:**
```
/etc/logrotate.d/samba
/etc/samba/lmhosts
/etc/samba/smb.conf
/etc/sysconfig/samba
```

---

## Samba Daemons

**smbd:**
*   File and printer sharing
*   Authentication
*   Service: `smb.service`

**nmbd:**
*   NetBIOS name service
*   Browsing
*   Service: `nmb.service`

**winbindd (optional):**
*   Domain integration
*   Service: `winbind.service`

---

## Starting Samba Services

### RHEL 7/8 and Fedora

**Enable at boot:**
```bash
systemctl enable smb.service
systemctl enable nmb.service
```

**Start services:**
```bash
systemctl start smb.service
systemctl start nmb.service
```

**Check status:**
```bash
systemctl status smb.service
systemctl status nmb.service
```

**Example output:**
```
● smb.service - Samba SMB Daemon
   Loaded: loaded (/usr/lib/systemd/system/smb.service; enabled)
   Active: active (running) since Mon 2019-09-02 16:45:00 EDT
   Main PID: 15678 (smbd)
   Status: "smbd: ready to serve connections..."
```

**Restart:**
```bash
systemctl restart smb.service
systemctl restart nmb.service
```

### RHEL 6 and Earlier

**Enable at boot:**
```bash
chkconfig smb on
chkconfig nmb on
```

**Start services:**
```bash
service smb start
service nmb start
```

---

## Basic Configuration

**Default smb.conf:**
```
[global]
    workgroup = WORKGROUP
    server string = Samba Server Version %v
    security = user
    passdb backend = tdbsam
```

**Minimal working config:**
```
[global]
    workgroup = WORKGROUP
    security = user

[share]
    path = /srv/samba/share
    browseable = yes
    writable = yes
    guest ok = no
```

---

## Testing Configuration

**Check syntax:**
```bash
testparm
```

**Example output:**
```
Load smb config files from /etc/samba/smb.conf
Processing section "[homes]"
Processing section "[printers]"
Loaded services file OK.
Server role: ROLE_STANDALONE
```

**Test specific file:**
```bash
testparm /etc/samba/smb.conf
```

**Verbose output:**
```bash
testparm -v
```

---

## Testing Samba

**Check if running:**
```bash
systemctl status smb.service nmb.service
```

**Check listening ports:**
```bash
netstat -tupln | grep mbd
```

**Expected output:**
```
tcp  0  0  0.0.0.0:445  0.0.0.0:*  LISTEN  15678/smbd
tcp  0  0  0.0.0.0:139  0.0.0.0:*  LISTEN  15678/smbd
udp  0  0  0.0.0.0:137  0.0.0.0:*          15679/nmbd
udp  0  0  0.0.0.0:138  0.0.0.0:*          15679/nmbd
```

**List shares:**
```bash
smbclient -L localhost -N
```

**Example output:**
```
Sharename       Type      Comment
---------       ----      -------
share           Disk      Shared Directory
IPC$            IPC       IPC Service
```

---

## Log Files

**Main logs:**
*   `/var/log/samba/log.smbd` - smbd log
*   `/var/log/samba/log.nmbd` - nmbd log

**Per-client logs:**
*   `/var/log/samba/log.%m` - %m = client name

**View logs:**
```bash
tail -f /var/log/samba/log.smbd
```

---

## Review Questions

1. What packages are needed for Samba?

2. Where is the main Samba configuration file?

3. What command tests smb.conf syntax?

4. What are the two main Samba services?

5. True or False: smbd handles NetBIOS name service.

6. What command lists Samba shares?

7. Where are Samba log files located?

8. What ports should be listening after starting Samba?

---

## Quick Reference

```
Installation:
└── yum install samba samba-client samba-common

Configuration:
└── /etc/samba/smb.conf

Services (RHEL 7+):
├── systemctl enable smb.service nmb.service
├── systemctl start smb.service nmb.service
├── systemctl status smb.service nmb.service
└── systemctl restart smb.service nmb.service

Services (RHEL 6):
├── chkconfig smb on
├── chkconfig nmb on
├── service smb start
└── service nmb start

Daemons:
├── smbd  → File/printer sharing (smb.service)
├── nmbd  → NetBIOS name service (nmb.service)
└── winbindd → Domain integration (winbind.service)

Testing:
├── testparm                  → Check syntax
├── testparm -v               → Verbose
├── smbclient -L localhost -N → List shares
└── netstat -tupln | grep mbd → Check ports

Ports:
├── TCP 445 → SMB
├── TCP 139 → NetBIOS Session
├── UDP 137 → NetBIOS Name
└── UDP 138 → NetBIOS Datagram

Logs:
├── /var/log/samba/log.smbd → smbd log
├── /var/log/samba/log.nmbd → nmbd log
└── /var/log/samba/log.%m   → Per-client

Basic Config:
[global]
├── workgroup = WORKGROUP
└── security = user

[share]
├── path = /srv/samba/share
├── browseable = yes
├── writable = yes
└── guest ok = no
```
