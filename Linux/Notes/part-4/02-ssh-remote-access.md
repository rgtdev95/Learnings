# SSH and Remote Access

## Secure Shell (SSH) Overview

**Purpose:** Encrypted remote access to Linux servers

**Replaces insecure tools:**
*   `telnet` → `ssh` (remote login)
*   `rcp` → `scp` (remote copy)
*   `rsh` → `ssh` (remote execution)
*   `ftp` → `sftp` (file transfer)

**Why SSH?**
*   All communication encrypted
*   Authentication encrypted
*   Prevents password sniffing
*   Prevents data interception

---

## SSH Packages

### RHEL/Fedora

```bash
yum list installed | grep openssh
```

**Packages:**
*   `openssh` - Core SSH files
*   `openssh-clients` - Client tools (ssh, scp, sftp)
*   `openssh-server` - Server daemon (sshd)

### Ubuntu

```bash
dpkg --list | grep openssh
```

**Packages:**
*   `openssh-client` - Client tools (installed by default)
*   `openssh-server` - Server daemon (install separately)

```bash
sudo apt-get install openssh-server
```

---

## Starting SSH Server

### Check Status

**RHEL 6:**
```bash
chkconfig --list sshd
```

**RHEL 7+/Fedora:**
```bash
systemctl status sshd.service
```

**Ubuntu:**
```bash
systemctl status ssh.service
```

### Start SSH Service

**RHEL 6:**
```bash
service sshd start
```

**RHEL 7+/Fedora:**
```bash
systemctl start sshd.service
```

**Ubuntu:**
```bash
systemctl start ssh.service
```

### Enable at Boot

**RHEL 6:**
```bash
chkconfig sshd on
```

**RHEL 7+/Fedora:**
```bash
systemctl enable sshd.service
```

**Ubuntu:**
```bash
systemctl enable ssh.service
```

---

## SSH Server Configuration

**Config file:** `/etc/ssh/sshd_config`

### Critical Security Setting

**Disable root login:**
```bash
# Edit /etc/ssh/sshd_config
PermitRootLogin no
```

**Restart after changes:**
```bash
systemctl restart sshd
```

**Why?**
*   Forces users to log in as regular user
*   Must use `su` or `sudo` to become root
*   Adds security layer

---

## SSH Client Tools

### ssh - Remote Login

**Basic syntax:**
```bash
ssh username@hostname
ssh username@IP_address
```

**Example:**
```bash
ssh johndoe@10.140.67.23
```

**First-time connection:**
*   Prompted to accept host key
*   Type `yes` to continue
*   Host key saved to `~/.ssh/known_hosts`

**Subsequent connections:**
*   No prompt (key already trusted)
*   If key changes, connection refused (security warning)

**Exit remote session:**
```bash
exit
```

Or type: `~.` (tilde-period)

---

### ssh - Remote Execution

**Run single command:**
```bash
ssh user@host command
```

**Examples:**
```bash
# Get hostname
ssh johndoe@10.140.67.23 hostname

# View file
ssh johndoe@10.140.67.23 "cat myfile"

# Multiple commands (use quotes!)
ssh johndoe@10.140.67.23 "ls -l /tmp"
```

**Note:** Relative paths are relative to user's home directory

---

### ssh - X11 Forwarding

**Run graphical apps remotely:**

**Enable on server:**
```bash
# In /etc/ssh/sshd_config
X11Forwarding yes
```

**Use from client:**
```bash
# Single app
ssh -X user@host system-config-printer

# Multiple apps
ssh -X user@host
$ gedit &
$ firefox &
$ exit
```

**Apps display on local screen, run on remote server!**

---

### scp - Secure Copy

**Copy files between systems**

**Syntax:**
```bash
scp source destination
```

**Local to remote:**
```bash
scp /home/chris/memo johndoe@10.140.67.23:/tmp
```

**Remote to local:**
```bash
scp johndoe@10.140.67.23:/tmp/file.txt /home/chris/
```

**Recursive copy:**
```bash
scp -r johndoe@10.140.67.23:/usr/share/man/man1/ /tmp/
```

**Limitations:**
*   Doesn't preserve all attributes
*   Symbolic links copied as files
*   Copies files even if already present

---

### rsync - Better Backup Tool

**Advantages over scp:**
*   Preserves permissions and timestamps
*   Preserves symbolic links
*   Only copies changed files
*   Can mirror directories

**Basic syntax:**
```bash
rsync -avl source destination
```

**Options:**
*   `-a` - Archive mode (recursive, preserve attributes)
*   `-v` - Verbose
*   `-l` - Copy symbolic links as links

**Examples:**
```bash
# Initial copy
rsync -avl user@host:/usr/share/man/man1/ /tmp/

# Run again - nothing copied (already there!)
rsync -avl user@host:/usr/share/man/man1/ /tmp/

# Mirror (delete local files not on remote)
rsync -avl --delete user@host:/source/ /dest/
```

---

### sftp - Interactive File Transfer

**FTP-style interface over SSH**

**Start session:**
```bash
sftp johndoe@jd.example.com
```

**Commands:**
*   `ls` - List remote files
*   `cd` - Change remote directory
*   `pwd` - Print remote working directory
*   `get file` - Download file
*   `put file` - Upload file
*   `quit` - Exit

**Example:**
```bash
sftp> ls
sftp> cd documents
sftp> get report.pdf
sftp> put myfile.txt
sftp> quit
```

---

## Key-Based Authentication

**Benefits:**
*   No password needed
*   More secure (if key protected)
*   Enables automation (scripts, cron)

### Setup Process

**1. Generate key pair (on client):**
```bash
ssh-keygen
```

**Prompts:**
*   File location: Press Enter (default: `~/.ssh/id_rsa`)
*   Passphrase: Press Enter twice (no passphrase)

**Creates:**
*   `~/.ssh/id_rsa` - Private key (keep secret!)
*   `~/.ssh/id_rsa.pub` - Public key (share this)

**2. Copy public key to server:**
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
```

**3. Test passwordless login:**
```bash
ssh user@host
# No password prompt!
```

### How It Works

1. Client has private key
2. Server has public key (in `~/.ssh/authorized_keys`)
3. Keys match → access granted
4. No password needed!

---

## Disable Password Authentication

**After keys are set up:**

**Edit `/etc/ssh/sshd_config`:**
```bash
PasswordAuthentication no
```

**Restart sshd:**
```bash
systemctl restart sshd
```

**Result:**
*   Only key-based auth allowed
*   No password login possible
*   More secure!

---

## SSH Files and Locations

**Client files:**
*   `~/.ssh/id_rsa` - Private key
*   `~/.ssh/id_rsa.pub` - Public key
*   `~/.ssh/known_hosts` - Trusted server keys

**Server files:**
*   `/etc/ssh/sshd_config` - Server config
*   `/etc/ssh/ssh_host_*` - Server keys
*   `~/.ssh/authorized_keys` - Allowed client keys

---

## Review Questions

1. What command logs in to a remote system via SSH?

2. What file contains SSH server configuration?

3. How do you copy a file from local to remote with scp?

4. What's the advantage of rsync over scp?

5. True or False: sftp uses the FTP protocol.

6. What command generates SSH key pairs?

7. Where is the public key stored on the server?

8. What setting disables root SSH login?

---

## Quick Reference

```
SSH Service:
├── systemctl status sshd    → Check status
├── systemctl start sshd     → Start service
├── systemctl enable sshd    → Enable at boot
└── /etc/ssh/sshd_config     → Server config

Remote Login:
├── ssh user@host            → Login
├── ssh user@host command    → Remote execution
└── ssh -X user@host         → X11 forwarding

File Transfer:
├── scp file user@host:/path → Copy file
├── scp -r dir user@host:/   → Copy directory
├── rsync -avl src dest      → Sync files
└── sftp user@host           → Interactive FTP

Key-Based Auth:
├── ssh-keygen               → Generate keys
├── ssh-copy-id user@host    → Copy public key
└── ssh user@host            → Passwordless login

Security:
├── PermitRootLogin no       → Disable root login
├── PasswordAuthentication no → Require keys only
└── ~/.ssh/authorized_keys   → Allowed public keys

Common Options:
├── -r  → Recursive (scp)
├── -a  → Archive mode (rsync)
├── -v  → Verbose
├── -l  → Preserve links (rsync)
└── -X  → X11 forwarding (ssh)
```
