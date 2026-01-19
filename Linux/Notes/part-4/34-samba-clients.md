# Samba Clients

## Accessing from Windows

### Windows 10/11

**File Explorer method:**
1. Open File Explorer
2. Type in address bar: `\\servername\sharename`
3. Or: `\\192.168.1.100\sharename`
4. Enter credentials when prompted

**Map network drive:**
1. Right-click "This PC"
2. Select "Map network drive"
3. Choose drive letter
4. Enter path: `\\servername\sharename`
5. Check "Reconnect at sign-in"
6. Click "Finish"
7. Enter credentials

**Command line:**
```cmd
net use Z: \\servername\sharename
net use Z: \\servername\sharename /user:username password
```

### Windows 7/8

**Network browsing:**
1. Open Network
2. Find server name
3. Double-click server
4. See available shares
5. Double-click share
6. Enter credentials

---

## Accessing from Linux

### smbclient Command

**List shares:**
```bash
smbclient -L servername -U username
```

**Anonymous list:**
```bash
smbclient -L servername -N
```

**Connect to share:**
```bash
smbclient //servername/sharename -U username
```

**Example session:**
```bash
$ smbclient //fileserver/share -U chris
Password:
smb: \> ls
smb: \> cd directory
smb: \> get file.txt
smb: \> put localfile.txt
smb: \> quit
```

### smbclient Commands

**List files:**
```
smb: \> ls
smb: \> dir
```

**Change directory:**
```
smb: \> cd directory
smb: \> cd ..
```

**Download file:**
```
smb: \> get filename
smb: \> get remote.txt local.txt
```

**Upload file:**
```
smb: \> put filename
smb: \> put local.txt remote.txt
```

**Multiple downloads:**
```
smb: \> mget *.txt
```

**Multiple uploads:**
```
smb: \> mput *.txt
```

**Create directory:**
```
smb: \> mkdir dirname
```

**Delete file:**
```
smb: \> del filename
```

**Help:**
```
smb: \> help
smb: \> help command
```

**Quit:**
```
smb: \> quit
```

---

## Mounting Samba Shares

### Temporary Mount

**Mount command:**
```bash
mount -t cifs //servername/sharename /mnt/share -o username=chris
```

**With credentials:**
```bash
mount -t cifs //servername/sharename /mnt/share \
  -o username=chris,password=mypass
```

**Guest mount:**
```bash
mount -t cifs //servername/public /mnt/public -o guest
```

**Required package:**
```bash
yum install cifs-utils
```

### Permanent Mount

**Create credentials file:**
```bash
vi /root/.smbcredentials
```

**Contents:**
```
username=chris
password=mypassword
domain=WORKGROUP
```

**Secure file:**
```bash
chmod 600 /root/.smbcredentials
```

**Add to /etc/fstab:**
```
//servername/sharename /mnt/share cifs credentials=/root/.smbcredentials,uid=1000,gid=1000 0 0
```

**Mount:**
```bash
mount -a
```

**Verify:**
```bash
df -h | grep share
```

---

## Graphical File Managers

### GNOME Files (Nautilus)

**Connect to server:**
1. Files → Other Locations
2. "Connect to Server" at bottom
3. Enter: `smb://servername/sharename`
4. Click "Connect"
5. Enter credentials

**Bookmark:**
*   Drag share to sidebar
*   Access anytime

### KDE Dolphin

**Connect:**
1. Location bar: `smb://servername/sharename`
2. Enter credentials
3. Browse files

---

## Command-Line File Operations

**Copy to share:**
```bash
smbclient //server/share -U user -c "put localfile.txt"
```

**Copy from share:**
```bash
smbclient //server/share -U user -c "get remotefile.txt"
```

**Execute multiple commands:**
```bash
smbclient //server/share -U user -c "cd dir; mget *.txt"
```

---

## Troubleshooting Connections

**Cannot connect:**
*   Check if smbd is running
*   Check firewall (ports 445, 139)
*   Verify share exists: `testparm -s`
*   Check network connectivity: `ping servername`

**Authentication failed:**
*   Verify username exists: `pdbedit -L`
*   Check password: `smbpasswd username`
*   Check valid users in smb.conf
*   Check /etc/samba/smbusers

**Permission denied:**
*   Check Linux file permissions
*   Check SELinux contexts
*   Check writable setting in smb.conf
*   Check valid users / write list

**Mount fails:**
*   Install cifs-utils: `yum install cifs-utils`
*   Check credentials file permissions (600)
*   Verify share path
*   Check mount point exists

**Slow performance:**
*   Check network speed
*   Check server load
*   Disable oplocks if needed
*   Check for antivirus interference

---

## Testing Tools

**Check if server is reachable:**
```bash
ping servername
```

**List shares:**
```bash
smbclient -L servername -N
```

**Test authentication:**
```bash
smbclient //servername/sharename -U username
```

**Check mounted shares:**
```bash
mount | grep cifs
df -h | grep cifs
```

**View Samba status:**
```bash
smbstatus
```

---

## Review Questions

1. How do you map a network drive in Windows?

2. What command lists Samba shares from Linux?

3. What package is needed to mount CIFS shares?

4. Where should credentials be stored for permanent mounts?

5. True or False: smbclient uses FTP-like commands.

6. What is the format for accessing a share in Windows?

7. How do you mount a Samba share temporarily?

8. What command shows connected Samba users?

---

## Quick Reference

```
Windows Access:
├── File Explorer: \\servername\sharename
├── Map drive: net use Z: \\servername\sharename
└── Browse: Network → servername

Linux smbclient:
├── smbclient -L server -U user  → List shares
├── smbclient //server/share -U user → Connect
├── ls, cd, get, put             → Commands
└── quit                         → Exit

Mounting (Temporary):
└── mount -t cifs //server/share /mnt -o username=user

Mounting (Permanent):
1. Create /root/.smbcredentials:
   username=user
   password=pass
2. chmod 600 /root/.smbcredentials
3. Add to /etc/fstab:
   //server/share /mnt cifs credentials=/root/.smbcredentials 0 0
4. mount -a

GNOME Files:
├── Files → Other Locations
├── Connect to Server
└── smb://servername/sharename

Required Package:
└── yum install cifs-utils

Troubleshooting:
├── ping servername
├── smbclient -L servername -N
├── testparm -s
├── smbstatus
└── mount | grep cifs

Common Issues:
├── Cannot connect → Check firewall, service
├── Auth failed → Check user, password
├── Permission denied → Check permissions, SELinux
└── Mount fails → Install cifs-utils, check credentials
```
