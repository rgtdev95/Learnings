# FTP Basics

## File Transfer Protocol (FTP)

**Purpose:** Transfer files between computers over a network

**History:**
*   One of the oldest Internet protocols
*   Defined in RFC 959 (1985)
*   Still widely used today

**Use cases:**
*   Website file uploads
*   Software distribution
*   File sharing
*   Backup transfers

---

## FTP Protocol Overview

**Ports:**
*   **Port 21** - Control connection (commands)
*   **Port 20** - Data connection (file transfers)

**Authentication:**
*   Username and password
*   Anonymous access (optional)

**Commands:**
*   `USER` - Send username
*   `PASS` - Send password
*   `LIST` - List files
*   `RETR` - Download file
*   `STOR` - Upload file
*   `QUIT` - Disconnect

---

## Active vs Passive FTP

### Active FTP (PORT)

**Process:**
1. Client connects to server port 21
2. Client opens random port (>1024)
3. Client sends PORT command with its IP and port
4. Server connects back to client for data transfer

**Problem:**
*   Client firewall blocks incoming connections
*   NAT issues

**Diagram:**
```
Client → Server:21 (control)
Server:20 → Client:random (data)
```

### Passive FTP (PASV)

**Process:**
1. Client connects to server port 21
2. Client sends PASV command
3. Server opens random port and sends to client
4. Client connects to server's random port for data

**Benefits:**
*   Works through firewalls
*   Works with NAT
*   Recommended for Internet use

**Diagram:**
```
Client → Server:21 (control)
Client → Server:random (data)
```

---

## Security Concerns

**FTP weaknesses:**
*   **Unencrypted** - Passwords sent in clear text
*   **Unencrypted data** - Files transferred unencrypted
*   **Port 21 attacks** - Well-known attack target

**Alternatives:**
*   **SFTP** - SSH File Transfer Protocol (encrypted)
*   **FTPS** - FTP over SSL/TLS (encrypted)
*   **SCP** - Secure Copy (encrypted)
*   **rsync over SSH** - Efficient sync (encrypted)

**When to use FTP:**
*   Internal networks only
*   Public file distribution (anonymous)
*   Legacy systems requiring FTP

---

## Anonymous FTP

**Purpose:** Public file access without authentication

**Username:** `anonymous` or `ftp`

**Password:** Email address (convention, not enforced)

**Use cases:**
*   Software downloads
*   Public file repositories
*   Open data sharing

**Security:**
*   Read-only access (typically)
*   Limited to specific directory
*   No system access

---

## vsftpd Overview

**What is vsftpd?**
*   Very Secure FTP Daemon
*   Most popular FTP server for Linux
*   Focus on security and performance

**Features:**
*   Virtual users
*   Virtual IP configurations
*   Bandwidth throttling
*   Per-user configuration
*   IPv6 support
*   Encryption support (SSL/TLS)

**Why vsftpd?**
*   Secure by default
*   Fast and efficient
*   Easy to configure
*   Well-maintained

---

## FTP vs SFTP vs FTPS

**FTP (File Transfer Protocol):**
*   Port 21 (control), 20 (data)
*   Unencrypted
*   Original protocol

**SFTP (SSH File Transfer Protocol):**
*   Port 22
*   Encrypted via SSH
*   Different protocol (not FTP over SSH)
*   Recommended for security

**FTPS (FTP over SSL/TLS):**
*   Port 21 (can use others)
*   Encrypted via SSL/TLS
*   Extension of FTP
*   Two modes: Explicit, Implicit

---

## FTP Connection Modes

**ASCII mode:**
*   Text files
*   Converts line endings
*   Windows ↔ Unix conversion

**Binary mode:**
*   All other files
*   No conversion
*   Exact byte-for-byte transfer

**Auto mode:**
*   Client decides based on file type
*   Usually default

---

## Review Questions

1. What ports does FTP use?

2. What's the difference between active and passive FTP?

3. Why is FTP considered insecure?

4. What is vsftpd?

5. True or False: SFTP is FTP over SSH.

6. What username is used for anonymous FTP?

7. Which FTP mode should be used for images?

8. What is the main advantage of passive FTP?

---

## Quick Reference

```
FTP Overview:
├── Protocol: File Transfer Protocol
├── Port 21: Control connection
├── Port 20: Data connection (active mode)
└── RFC 959 (1985)

Active FTP:
├── Client → Server:21 (control)
├── Server:20 → Client:random (data)
└── Problem: Firewall/NAT issues

Passive FTP:
├── Client → Server:21 (control)
├── Client → Server:random (data)
└── Recommended for Internet

Security Issues:
├── Unencrypted passwords
├── Unencrypted data
└── Well-known attack target

Secure Alternatives:
├── SFTP (SSH File Transfer Protocol) - Port 22
├── FTPS (FTP over SSL/TLS) - Port 21
├── SCP (Secure Copy)
└── rsync over SSH

Anonymous FTP:
├── Username: anonymous or ftp
├── Password: email (convention)
└── Use: Public file distribution

vsftpd:
├── Very Secure FTP Daemon
├── Most popular Linux FTP server
├── Secure by default
└── Fast and efficient

Transfer Modes:
├── ASCII: Text files (line ending conversion)
├── Binary: All other files (no conversion)
└── Auto: Client decides

FTP vs SFTP vs FTPS:
├── FTP: Port 21, unencrypted
├── SFTP: Port 22, SSH encrypted (different protocol)
└── FTPS: Port 21, SSL/TLS encrypted
```
