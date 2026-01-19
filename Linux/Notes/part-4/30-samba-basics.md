# Samba Basics

## SMB/CIFS Protocol

**SMB (Server Message Block):**
*   File and printer sharing protocol
*   Developed by Microsoft
*   Used by Windows networks

**CIFS (Common Internet File System):**
*   Enhanced version of SMB
*   Microsoft's implementation
*   Terms often used interchangeably

**Purpose:**
*   Share files between Windows/Linux
*   Share printers
*   Provide authentication
*   Browse network resources

---

## Samba Overview

**What is Samba?**
*   Open-source implementation of SMB/CIFS
*   Allows Linux to act as Windows file/print server
*   Allows Linux to access Windows shares

**History:**
*   Created by Andrew Tridgell (1992)
*   Reverse-engineered SMB protocol
*   Now supports modern Windows features

**Use cases:**
*   Linux file server for Windows clients
*   Linux accessing Windows shares
*   Mixed Windows/Linux environments
*   Replacing Windows file servers

---

## Samba Components

**Main daemons:**

**smbd:**
*   Handles file and printer sharing
*   Authentication
*   Port 445 (TCP)
*   Port 139 (TCP, legacy)

**nmbd:**
*   NetBIOS name service
*   WINS server
*   Browsing service
*   Port 137 (UDP)
*   Port 138 (UDP)

**winbindd (optional):**
*   Windows domain integration
*   Maps Windows users to Linux
*   Active Directory support

---

## Workgroups vs Domains

### Workgroups

**Characteristics:**
*   Peer-to-peer network
*   No centralized authentication
*   Each computer has own user database
*   Simple setup

**Use cases:**
*   Small networks (< 10 computers)
*   Home networks
*   Simple file sharing

**Example:**
*   Workgroup name: `WORKGROUP`
*   Each computer authenticates locally

### Domains

**Characteristics:**
*   Centralized authentication
*   Domain controller manages users
*   Single sign-on
*   Group policies

**Types:**
*   **NT4 Domain** - Legacy Windows domain
*   **Active Directory** - Modern Windows domain

**Samba roles:**
*   Domain member
*   Domain controller (Samba 4+)

---

## NetBIOS Names

**Purpose:** Computer identification on network

**Characteristics:**
*   15 characters maximum
*   Case-insensitive
*   Usually same as hostname
*   Resolved to IP address

**Name types:**
*   Computer name
*   Workgroup name
*   Domain name

**Resolution methods:**
1. Broadcast
2. WINS server
3. lmhosts file
4. DNS (if configured)

---

## Ports Used by Samba

**TCP ports:**
*   **139** - NetBIOS Session Service (legacy)
*   **445** - Microsoft-DS (modern SMB)

**UDP ports:**
*   **137** - NetBIOS Name Service
*   **138** - NetBIOS Datagram Service

**Firewall requirements:**
*   Open these ports for Samba to work
*   Port 445 is primary for modern Windows

---

## Samba Features

**File sharing:**
*   Share Linux directories to Windows
*   Access Windows shares from Linux
*   Permissions mapping

**Printer sharing:**
*   Share Linux printers to Windows
*   CUPS integration
*   Windows driver support

**Authentication:**
*   User-level security
*   Share-level security (deprecated)
*   Domain authentication
*   Active Directory integration

**Advanced features:**
*   Access Control Lists (ACLs)
*   Recycle bin
*   Shadow copies
*   Quota support

---

## Samba vs NFS

**Samba (SMB/CIFS):**
*   Windows native
*   Works on Windows/Linux/Mac
*   More overhead
*   Better for mixed environments

**NFS (Network File System):**
*   UNIX/Linux native
*   Lighter weight
*   Better performance
*   Better for Linux-only environments

**When to use Samba:**
*   Windows clients
*   Mixed environment
*   Need Windows compatibility

**When to use NFS:**
*   Linux-only environment
*   Better performance needed
*   UNIX-style permissions

---

## Review Questions

1. What does SMB stand for?

2. What are the two main Samba daemons?

3. What port does modern SMB use?

4. What's the difference between a workgroup and a domain?

5. True or False: NetBIOS names can be 20 characters.

6. What daemon handles file sharing in Samba?

7. What is winbindd used for?

8. What ports must be open for Samba?

---

## Quick Reference

```
SMB/CIFS:
├── SMB: Server Message Block
├── CIFS: Common Internet File System
└── Purpose: File/printer sharing

Samba:
├── Open-source SMB/CIFS implementation
├── Linux as Windows file server
└── Linux accessing Windows shares

Daemons:
├── smbd    → File/printer sharing (TCP 445, 139)
├── nmbd    → NetBIOS name service (UDP 137, 138)
└── winbindd → Windows domain integration (optional)

Ports:
├── TCP 139 → NetBIOS Session (legacy)
├── TCP 445 → Microsoft-DS (modern)
├── UDP 137 → NetBIOS Name Service
└── UDP 138 → NetBIOS Datagram

Workgroups:
├── Peer-to-peer
├── No central authentication
├── Simple setup
└── Small networks

Domains:
├── Centralized authentication
├── Domain controller
├── Single sign-on
└── Group policies

NetBIOS Names:
├── 15 characters max
├── Case-insensitive
├── Computer identification
└── Resolved to IP

Features:
├── File sharing
├── Printer sharing
├── Authentication
├── ACLs
└── Active Directory support

Samba vs NFS:
├── Samba: Windows native, mixed environments
└── NFS: Linux native, better performance
```
