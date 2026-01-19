# Intrusion Detection

## Detecting Viruses

### Understanding Viruses

**What is a virus:**
*   Malicious software
*   Attaches to installed software
*   Can spread through media/networks
*   Self-replicating

**Linux virus misconception:**
*   "Linux has no viruses" - FALSE
*   Fewer than Windows, but they exist
*   Linux servers often serve Windows clients
*   Must scan for Windows viruses too

---

### Virus Signatures

**How antivirus works:**

**Virus signature:**
*   Hash created from virus binary code
*   Uniquely identifies that virus
*   Stored in signature database

**Scanning process:**
1. Scan files on system
2. Create hash of each file
3. Compare against signature database
4. Match = virus detected

**Database updates:**
*   Updated frequently
*   New threats discovered daily
*   Critical to keep current

---

### ClamAV Antivirus

**Installation:**
```bash
# Fedora/RHEL
dnf install clamav

# Ubuntu
apt-cache search clamav  # Review packages
apt-get install clamav
```

**Features:**
*   Open source and free
*   Scans Linux and Windows viruses
*   Command-line and daemon modes
*   Regular signature updates

**More info:** clamav.net

---

## Detecting Rootkits

### Understanding Rootkits

**What is a rootkit:**
*   Malicious program collection
*   Hides itself
*   Maintains high-level access
*   Circumvents detection software

**How it works:**
*   Replaces system commands
*   Replaces system programs
*   Hides processes
*   Covers its tracks

**Purpose:**
*   Get root-level access
*   Maintain root-level access
*   Stay hidden

**Name origin:**
*   **root** - Administrator access required
*   **kit** - Collection of programs working together

---

### chkrootkit

**Installation:**
```bash
# Fedora/RHEL
yum install chkrootkit

# Ubuntu
apt-get install chkrootkit
```

**Run scan:**
```bash
chkrootkit
```

**Search for infections:**
```bash
chkrootkit | grep INFECTED
```

**Example output:**
```
Checking 'du'... INFECTED
Checking 'find'... INFECTED
Checking 'ls'... INFECTED
Checking 'lsof'... INFECTED
Checking 'pstree'... INFECTED
Searching for Suckit rootkit... Warning: /sbin/init INFECTED
```

**Infected files:**
*   Common bash commands (du, find, ls)
*   System utilities (lsof, pstree)
*   Init system (/sbin/init)

**False positives:**
*   Common with detection tools
*   Verify before taking action
*   Known bugs can cause false alarms
*   Example: Suckit detection above is false positive

**Best practice:**
*   Run from Live CD/USB
*   Prevents rootkit from hiding itself
*   More reliable results

**Fedora Security Spin:**
*   Live CD with security tools
*   Includes chkrootkit
*   Download: labs.fedoraproject.org/en/security

**More info:** chkrootkit.org

---

### Rootkit Hunter (rkhunter)

**Alternative rootkit detector:**

**Installation:**
```bash
dnf install rkhunter
apt-get install rkhunter
```

**Run scan:**
```bash
rkhunter -c
```

**Configuration:**
*   `/etc/rkhunter.conf`
*   Customize scan settings
*   Set email alerts
*   Schedule automatic scans

**Features:**
*   Checks for rootkits
*   Verifies file integrity
*   Detects hidden files
*   Checks network ports

---

## Intrusion Detection Systems (IDS)

### IDS Overview

**What is IDS:**
*   Monitors system activities
*   Detects potential malicious activities
*   Reports suspicious behavior
*   Can prevent intrusions (IPS)

**Types:**
*   **IDS** - Intrusion Detection System (detects)
*   **IPS** - Intrusion Prevention System (prevents)
*   **IDPS** - Combined detection and prevention

---

### Popular Linux IDS Tools

**aide (Advanced Intrusion Detection Environment):**

**Installation:**
```bash
dnf install aide
apt-get install aide
```

**How it works:**
1. Create initial database ("first picture")
2. Later create new database ("second picture")
3. Compare databases
4. Report differences

**Initialize database:**
```bash
aide -i
```

**Output:**
```
AIDE, version 0.16.11
### AIDE database at /var/lib/aide/aide.db.new.gz initialized.
```

**Move database to correct location:**
```bash
cp /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
```

**Check for changes:**
```bash
aide -C
```

**Example output:**
```
File: /bin/find
Size     : 189736                       , 4620
Ctime    : 2020-02-10 13:00:44          , 2020-02-11 03:05:52
MD5      : <NONE>                       , rUJj8NtNa1v4nmV5zfoOjg==
SHA256   : <NONE>                       , jg60Soawj4S/UZXm5h4aEGJ+xZgGwCmN

File: /bin/ls
Size     : 112704                       , 6122
Ctime    : 2020-02-10 13:04:57          , 2020-02-11 03:05:52
MD5      : POeOop46MvRx9qfEoYTXOQ==     , IShMBpbSOY8axhw1Kj8Wdw==
```

**Configuration:**
*   `/etc/aide.conf`
*   Define what to check
*   Set database locations
*   Configure comparison rules

**More info:** aide.sourceforge.net

---

**Snort:**

**Features:**
*   Network intrusion detection
*   Real-time traffic analysis
*   Packet logging
*   Protocol analysis

**Installation:**
*   RPM or tarball from website
*   More complex setup
*   Powerful capabilities

**More info:** snort.org

---

**tripwire:**

**Installation:**
```bash
dnf install tripwire
apt-get install tripwire
```

**Features:**
*   File integrity monitoring
*   Similar to aide
*   Commercial and open source versions

**Note:**
*   No longer fully open source
*   Original code still available
*   Check website for details

**More info:** tripwire.org

---

## Penetration Testing

### What is Penetration Testing

**Also called:**
*   Pen testing
*   Ethical hacking

**Purpose:**
*   Test system security
*   Simulate malicious attacks
*   Find vulnerabilities
*   Verify security measures

---

### Kali Linux

**What is Kali:**
*   Linux distribution for pen testing
*   Hundreds of security tools included
*   Can run from Live DVD/USB
*   Professional security testing platform

**Features:**
*   Pre-installed security tools
*   Network analysis tools
*   Vulnerability scanners
*   Exploitation frameworks
*   Forensics tools

**Download:** www.kali.org

**Training:**
*   Offensive Security offers training
*   Professional certifications
*   Hands-on courses
*   Website: www.offensive-security.com

---

## Security Reviews and Audits

### Compliance Reviews

**What it is:**
*   Audit of computer system environment
*   Verify policies are followed
*   Check procedures are correct
*   Ensure compliance

**Methods:**
*   Internal audits
*   External audits
*   Penetration testing
*   Automated scanning

---

### Security Reviews

**What it is:**
*   Audit of policies and procedures
*   Verify best security practices
*   Update outdated policies
*   Improve security posture

**Stay informed:**

**CISA (Cybersecurity and Infrastructure Security Agency):**
*   URL: www.us-cert.gov
*   National Cyber Alert System
*   RSS feeds on threats
*   Security advisories

**SANS Institute:**
*   URL: www.sans.org/security-resources
*   Computer Security Research newsletters
*   RSS feeds on threats
*   Training and certifications

**Gibson Research Corporation:**
*   URL: www.grc.com
*   Security Now! netcast
*   Security tools
*   Research articles

---

## Review Questions

1. What is a virus signature?

2. What open source antivirus software is available for Linux?

3. What is a rootkit?

4. What tool detects rootkits on Linux?

5. What does IDS stand for?

6. How does aide detect intrusions?

7. What is Kali Linux used for?

8. True or False: Linux systems never need antivirus software.

---

## Quick Reference

```
Viruses:
├── Definition: Malicious self-replicating software
├── Signature: Hash identifying virus
├── Database: Collection of signatures
└── Scanning: Compare file hashes to database

ClamAV:
├── Install: dnf install clamav
├── Open source antivirus
├── Scans Linux and Windows viruses
└── Website: clamav.net

Rootkits:
├── Hides itself
├── Maintains root access
├── Replaces system commands
└── Circumvents detection

chkrootkit:
├── Install: yum install chkrootkit
├── Run: chkrootkit
├── Search: chkrootkit | grep INFECTED
├── Best: Run from Live CD
└── Website: chkrootkit.org

rkhunter:
├── Install: dnf install rkhunter
├── Run: rkhunter -c
├── Config: /etc/rkhunter.conf
└── Checks rootkits, file integrity, hidden files

IDS Tools:
├── aide     → File integrity monitoring
├── Snort    → Network intrusion detection
└── tripwire → File integrity monitoring

aide Commands:
├── aide -i  → Initialize database
├── cp /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
├── aide -C  → Check for changes
└── Config: /etc/aide.conf

aide Process:
1. aide -i                    → Create "first picture"
2. cp aide.db.new.gz aide.db.gz → Move database
3. aide -C                    → Check for changes

Penetration Testing:
├── Kali Linux → Security testing distribution
├── Live DVD/USB available
├── Hundreds of security tools
├── Download: www.kali.org
└── Training: www.offensive-security.com

Security Resources:
├── CISA: www.us-cert.gov
├── SANS: www.sans.org/security-resources
└── GRC: www.grc.com

Review Types:
├── Compliance Review → Verify policies followed
└── Security Review → Audit policies/procedures

Best Practices:
├── Regular antivirus scans
├── Run rootkit detectors from Live CD
├── Use IDS for file integrity
├── Perform penetration testing
├── Stay informed on threats
├── Regular security audits
└── Update security policies
```
