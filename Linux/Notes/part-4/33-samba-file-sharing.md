# Samba File Sharing

## smb.conf Structure

**Location:** `/etc/samba/smb.conf`

**Sections:**
*   `[global]` - Server-wide settings
*   `[sharename]` - Individual share definitions

**Format:**
```
[section]
    parameter = value
```

**Comments:** Lines starting with `#` or `;`

---

## Global Section

**Basic settings:**
```
[global]
    workgroup = WORKGROUP
    server string = Samba Server %v
    netbios name = FILESERVER
    security = user
    passdb backend = tdbsam
```

**Parameters explained:**
*   `workgroup` - Workgroup/domain name
*   `server string` - Description (%v = version)
*   `netbios name` - Server NetBIOS name
*   `security` - Authentication mode
*   `passdb backend` - User database type

---

## Creating File Shares

**Basic share:**
```
[share]
    comment = Shared Directory
    path = /srv/samba/share
    browseable = yes
    writable = yes
    guest ok = no
```

**Read-only share:**
```
[docs]
    comment = Documentation
    path = /srv/samba/docs
    browseable = yes
    writable = no
    guest ok = yes
```

**Private share:**
```
[private]
    comment = Private Files
    path = /srv/samba/private
    browseable = no
    writable = yes
    valid users = chris
```

---

## Share Parameters

**Essential parameters:**

**path:**
*   Directory to share
*   Must exist
*   Example: `path = /srv/samba/share`

**comment:**
*   Description of share
*   Visible to users
*   Example: `comment = Team Files`

**browseable:**
*   `yes` - Visible in browse list
*   `no` - Hidden (must know name)

**writable:**
*   `yes` - Allow writes
*   `no` - Read-only
*   Alternative: `read only = no`

**guest ok:**
*   `yes` - Allow guest access
*   `no` - Require authentication
*   Alternative: `public = yes`

---

## User and Group Access

**Allow specific users:**
```
[team]
    path = /srv/samba/team
    valid users = chris, jake, mary
```

**Allow group:**
```
[dept]
    path = /srv/samba/dept
    valid users = @sales
```

**Deny users:**
```
[share]
    path = /srv/samba/share
    invalid users = baduser
```

**Read/write lists:**
```
[mixed]
    path = /srv/samba/mixed
    read list = guest, @viewers
    write list = chris, @editors
```

---

## Share Permissions

**Force user/group:**
```
[share]
    path = /srv/samba/share
    force user = smbuser
    force group = smbgroup
```

**Create mask:**
```
[share]
    path = /srv/samba/share
    create mask = 0664
    directory mask = 0775
```

**Force create mode:**
```
[share]
    path = /srv/samba/share
    force create mode = 0664
    force directory mode = 0775
```

---

## Public Shares

**Anonymous access:**
```
[public]
    comment = Public Files
    path = /srv/samba/public
    browseable = yes
    writable = no
    guest ok = yes
    guest only = yes
```

**Public with uploads:**
```
[uploads]
    comment = Public Uploads
    path = /srv/samba/uploads
    browseable = yes
    writable = yes
    guest ok = yes
    create mask = 0666
    directory mask = 0777
```

**⚠️ Warning:** Public writable shares are risky

---

## Home Directories

**Enable home directories:**
```
[homes]
    comment = Home Directories
    browseable = no
    writable = yes
    valid users = %S
```

**Parameters:**
*   `%S` - Substituted with username
*   `browseable = no` - Don't list all homes
*   Each user sees only their home

**Access:**
*   User `chris` connects to `\\server\chris`
*   Maps to `/home/chris`

---

## Printer Sharing

**Share all printers:**
```
[printers]
    comment = All Printers
    path = /var/spool/samba
    browseable = no
    printable = yes
    guest ok = no
```

**Global printer settings:**
```
[global]
    load printers = yes
    cups options = raw
    printing = cups
```

---

## Advanced Share Options

**Hide dot files:**
```
[share]
    hide dot files = yes
```

**Veto files:**
```
[share]
    veto files = /*.exe/*.com/*.bat/
```

**Delete veto files:**
```
[share]
    delete veto files = yes
```

**Case sensitivity:**
```
[share]
    case sensitive = no
    preserve case = yes
```

---

## Testing Configuration

**Check syntax:**
```bash
testparm
```

**Test specific share:**
```bash
testparm -s share
```

**Reload configuration:**
```bash
systemctl reload smb.service
```

---

## Review Questions

1. Where is the Samba configuration file?

2. What section contains server-wide settings?

3. What parameter sets the share path?

4. How do you allow a group in valid users?

5. True or False: browseable = no makes share inaccessible.

6. What does %S represent in [homes]?

7. What command tests smb.conf syntax?

8. How do you create a read-only share?

---

## Quick Reference

```
Configuration:
└── /etc/samba/smb.conf

Structure:
├── [global]    → Server settings
└── [sharename] → Share definitions

Basic Share:
[share]
├── comment = Description
├── path = /srv/samba/share
├── browseable = yes
├── writable = yes
└── guest ok = no

User Access:
├── valid users = chris, jake
├── valid users = @groupname
├── invalid users = baduser
├── read list = guest
└── write list = chris

Permissions:
├── create mask = 0664
├── directory mask = 0775
├── force user = smbuser
└── force group = smbgroup

Public Share:
[public]
├── path = /srv/samba/public
├── browseable = yes
├── writable = no
├── guest ok = yes
└── guest only = yes

Home Directories:
[homes]
├── comment = Home Directories
├── browseable = no
├── writable = yes
└── valid users = %S

Printers:
[printers]
├── path = /var/spool/samba
├── browseable = no
├── printable = yes
└── guest ok = no

Testing:
├── testparm           → Check syntax
├── testparm -s share  → Test specific share
└── systemctl reload smb.service → Reload config

Variables:
├── %S → Share/username
├── %u → Username
├── %g → Primary group
└── %v → Samba version
```
