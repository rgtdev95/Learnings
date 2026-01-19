# FTP Clients

## Firefox FTP Access

**URL format:**
```
ftp://hostname
ftp://username@hostname
ftp://username:password@hostname
```

**Examples:**
```
ftp://ftp.example.com
ftp://chris@ftp.example.com
ftp://anonymous@ftp.example.com
```

**Features:**
*   Browse directories
*   Download files (click to download)
*   View text files
*   No upload capability

**⚠️ Security:** Avoid putting passwords in URL (visible in history)

---

## Command-Line ftp Client

**Connect to server:**
```bash
ftp hostname
```

**Connect with username:**
```bash
ftp username@hostname
```

**Example session:**
```bash
$ ftp ftp.example.com
Connected to ftp.example.com.
220 (vsFTPd 3.0.3)
Name (ftp.example.com:user): chris
331 Please specify the password.
Password:
230 Login successful.
ftp>
```

### Basic Commands

**List files:**
```
ftp> ls
ftp> dir
```

**Change directory:**
```
ftp> cd directory
ftp> cd ..
```

**Print working directory:**
```
ftp> pwd
```

**Download file:**
```
ftp> get filename
ftp> get remote.txt local.txt
```

**Upload file:**
```
ftp> put filename
ftp> put local.txt remote.txt
```

**Multiple downloads:**
```
ftp> mget *.txt
```

**Multiple uploads:**
```
ftp> mput *.txt
```

**Delete file:**
```
ftp> delete filename
```

**Create directory:**
```
ftp> mkdir dirname
```

**Remove directory:**
```
ftp> rmdir dirname
```

**Transfer mode:**
```
ftp> ascii
ftp> binary
```

**Quit:**
```
ftp> quit
ftp> bye
```

---

## lftp Client

**Features:**
*   More powerful than ftp
*   Tab completion
*   Bookmarks
*   Mirror directories
*   Background transfers

**Installation:**
```bash
yum install lftp
```

**Connect:**
```bash
lftp ftp://hostname
lftp ftp://username@hostname
```

**Or:**
```bash
lftp
lftp> open hostname
lftp> login username
```

### lftp Commands

**List files:**
```
lftp> ls
lftp> ls -la
```

**Change directory:**
```
lftp> cd directory
```

**Download file:**
```
lftp> get filename
```

**Upload file:**
```
lftp> put filename
```

**Mirror directory (download):**
```
lftp> mirror remotedir localdir
```

**Mirror directory (upload):**
```
lftp> mirror -R localdir remotedir
```

**Background transfer:**
```
lftp> get -c filename &
```

**View jobs:**
```
lftp> jobs
```

**Bookmarks:**
```
lftp> bookmark add name
lftp> bookmark list
lftp> bookmark edit
```

**Help:**
```
lftp> help
lftp> help command
```

**Quit:**
```
lftp> quit
```

---

## Graphical FTP Clients

### FileZilla

**Installation:**
```bash
yum install filezilla
```

**Features:**
*   Drag-and-drop interface
*   Site manager
*   Transfer queue
*   Directory comparison
*   SFTP support

**Connection:**
1. File → Site Manager
2. New Site
3. Enter host, port, protocol
4. Enter username, password
5. Connect

### gFTP

**Installation:**
```bash
yum install gftp
```

**Features:**
*   GTK+ interface
*   Bookmarks
*   FTP, SFTP, HTTP support
*   Transfer queue

---

## Anonymous FTP Access

**Username:** `anonymous` or `ftp`

**Password:** Email address (convention)

**Example:**
```bash
$ ftp ftp.example.com
Name: anonymous
Password: user@example.com
```

**Or:**
```bash
$ ftp anonymous@ftp.example.com
```

---

## Secure Alternatives

### SFTP (SSH File Transfer Protocol)

**Connect:**
```bash
sftp hostname
sftp username@hostname
```

**Commands:** Similar to ftp

**Benefits:**
*   Encrypted connection
*   Uses SSH (port 22)
*   No separate server needed

### SCP (Secure Copy)

**Copy to remote:**
```bash
scp file.txt user@host:/path/
```

**Copy from remote:**
```bash
scp user@host:/path/file.txt .
```

**Copy directory:**
```bash
scp -r directory user@host:/path/
```

### rsync over SSH

**Sync to remote:**
```bash
rsync -avz -e ssh /local/path/ user@host:/remote/path/
```

**Sync from remote:**
```bash
rsync -avz -e ssh user@host:/remote/path/ /local/path/
```

**Benefits:**
*   Only transfers differences
*   Preserves permissions
*   Encrypted

---

## FTP Troubleshooting

**Connection refused:**
*   Check if vsftpd is running
*   Check firewall
*   Verify port 21 open

**Login failed:**
*   Check username/password
*   Check /etc/vsftpd/ftpusers
*   Check /etc/vsftpd/user_list
*   Check SELinux Booleans

**Permission denied:**
*   Check file permissions
*   Check SELinux contexts
*   Check write_enable setting

**Passive mode fails:**
*   Check firewall passive ports
*   Check pasv_min_port/pasv_max_port
*   Check pasv_address setting

---

## Review Questions

1. What command connects to an FTP server?

2. What ftp command downloads a file?

3. What is the lftp mirror command used for?

4. What username is used for anonymous FTP?

5. True or False: Firefox can upload files via FTP.

6. What port does SFTP use?

7. What command copies files securely?

8. How do you switch to binary mode in ftp?

---

## Quick Reference

```
Command-Line ftp:
├── ftp hostname
├── ls, dir          → List files
├── cd directory     → Change directory
├── get filename     → Download
├── put filename     → Upload
├── mget *.txt       → Multiple download
├── binary           → Binary mode
└── quit             → Exit

lftp:
├── lftp ftp://hostname
├── mirror dir       → Download directory
├── mirror -R dir    → Upload directory
├── get -c file &    → Background download
├── bookmark add     → Save bookmark
└── quit             → Exit

Firefox:
└── ftp://hostname (browse only)

Graphical Clients:
├── FileZilla
└── gFTP

Anonymous FTP:
├── Username: anonymous or ftp
└── Password: email@example.com

Secure Alternatives:
├── SFTP: sftp hostname
├── SCP: scp file user@host:/path/
└── rsync: rsync -avz -e ssh /local/ user@host:/remote/

Troubleshooting:
├── Connection refused → Check service, firewall
├── Login failed → Check user lists, password
├── Permission denied → Check permissions, SELinux
└── Passive fails → Check firewall passive ports
```
