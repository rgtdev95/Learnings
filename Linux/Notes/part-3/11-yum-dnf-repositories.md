# YUM and DNF Repositories

## Understanding YUM/DNF

**YUM** = YellowDog Updater Modified
**DNF** = Dandified YUM (Next generation)

**Purpose:** Solve RPM dependency problems by using repositories

---

## Key Concept: Repositories

Instead of individual packages, think of **collections** of packages.

**Repository** = Directory of RPM packages + metadata

**Benefits:**
*   Distribution manages dependencies (not user)
*   Single command installs package + all dependencies
*   Automatic dependency resolution

---

## How YUM/DNF Works

### Basic Command Syntax

```bash
yum [options] command
dnf [options] command
```

**Note:** In RHEL 8+ and Fedora, `yum` → symlink to `dnf`

### Installation Process (5 Phases)

**1. Check `/etc/yum.conf`**
*   Read default settings
*   Set cache location, log file, etc.

**2. Check `/etc/yum.repos.d/*.repo`**
*   Find enabled repositories
*   Get repository URLs

**3. Download Metadata**
*   Get package lists from each repo
*   Store in `/var/cache/yum/`
*   Reuse until metadata expires

**4. Install Packages**
*   Download RPMs to cache
*   Run pre-install scripts
*   Copy files to filesystem
*   Run post-install scripts

**5. Update RPM Database**
*   Store package info in `/var/lib/rpm/`
*   Track installed packages

---

## YUM Configuration: /etc/yum.conf

**Location:** `/etc/yum.conf` (or `/etc/dnf/dnf.conf`)

**Example:**
```ini
[main]
gpgcheck=1
installonly_limit=3
clean_requirements_on_remove=True
best=True
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
plugins=1
```

### Important Settings

| Setting | Meaning |
|---------|---------|
| `gpgcheck=1` | Verify package signatures (recommended) |
| `installonly_limit=3` | Keep 3 versions of same package (kernels) |
| `clean_requirements_on_remove=True` | Remove unused dependencies |
| `best=True` | Always use newest version |
| `cachedir` | Where to store downloads |
| `keepcache=0` | Delete packages after install (0=yes) |
| `debuglevel=2` | Amount of log detail |
| `logfile` | Where to log actions |
| `plugins=1` | Enable  YUM plugins |

**View all options:** `man yum.conf`

---

## Repository Configuration

### Repository Files: /etc/yum.repos.d/

**Format:** `*.repo` files

**Example:** `/etc/yum.repos.d/myrepo.repo`

```ini
[myrepo]
name=My repository of software packages
baseurl=http://myrepo.example.com/pub/myrepo
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/MYOWNKEY
```

### Repository Entry Breakdown

| Field | Meaning |
|-------|---------|
| `[reponame]` | Unique repository ID |
| `name` | Human-readable description |
| `baseurl` | URL to repository directory |
| `enabled` | 1=active, 0=inactive |
| `gpgcheck` | 1=verify signatures, 0=don't verify |
| `gpgkey` | Location of GPG signing key |

### baseurl Protocols

```
http://example.com/repo    → Web server
ftp://ftp.example.com/repo  → FTP server
file:///mnt/repo            → Local directory
nfs://nfs.example.com/repo  → NFS share
```

---

## Third-Party Repositories

**Fedora/RHEL only include:**
*   Open-source software
*   Freely redistributable packages

**Third-party repos provide:**
*   Non-free software
*   Proprietary codecs
*   Commercial applications

### RPM Fusion (Fedora)

**Most popular third-party repo**

**Enable Free repository:**
```bash
sudo rpm -Uvh http://download1.rpmfusion.org/free/fedora/\
rpmfusion-free-release-stable.noarch.rpm
```

**Enable Nonfree repository:**
```bash
sudo rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/\
rpmfusion-nonfree-release-stable.noarch.rpm
```

**Contains:**
*   Multimedia codecs
*   Proprietary drivers
*   Software with patent issues

### EPEL (RHEL/CentOS)

**Extra Packages for Enterprise Linux**

**Enable:**
```bash
sudo dnf install epel-release
```

**Provides:**
*   Additional packages not in RHEL
*   Community-maintained software

---

## Repository Metadata

**Location in repo:** `/repodata/` directory

**Contents:**
*   Package lists
*   Dependency information
*   File lists
*   Package groups

**Cached locally:** `/var/cache/yum/`

**Metadata expiration:**
*   YUM: 6 hours (default)
*   DNF: 48 hours (default)
*   Change: Set `metadata_expire` in yum.conf

---

## YUM vs DNF

### Differences

| Feature | YUM | DNF |
|---------|-----|-----|
| **Default in** | RHEL 7 | RHEL 8+, Fedora 22+ |
| **Performance** | Slower | Faster |
| **Dependency solver** | Older | Better |
| **API** | Less strict | Strict API |
| **Python** | Python 2 | Python 3 |
| **Extensions** | Limited | Plugin system |

### Compatibility

**In RHEL 8+ / Fedora:**
```bash
yum install package      # Actually runs dnf
dnf install package      # Directly runs dnf
```

**Result:** Both commands work identically

---

## Repository Best Practices

### 1. Don't Over-Enable

*   Each repo adds metadata download time
*   More repos = slower yum commands
*   Only enable repos you need

### 2. Trust Your Sources

*   Stick to official repos when possible
*   Verify third-party repo reputation
*   Check GPG keys

### 3. Prioritize Repositories

*   Use `priority` plugin if needed
*   Prevent conflicts between repos

### 4. Keep Metadata Fresh

*   Run `dnf clean metadata` if stale
*   Run `dnf makecache` to refresh

---

## Review Questions

1. What does YUM stand for?

2. Where is the main YUM configuration file located?

3. What directory contains repository configuration files?

4. What does `gpgcheck=1` mean?

5. True or False: All repositories must be web-based (HTTP).

6. Where does YUM cache downloaded metadata?

7. What is the most popular third-party repository for Fedora?

8. In RHEL 8, what command actually runs when you type `yum`?

---

## Quick Reference

```
YUM/DNF Architecture:
├── /etc/yum.conf          → Main config
├── /etc/yum.repos.d/      → Repository configs
├── /var/cache/yum/        → Downloaded metadata
└── /var/lib/rpm/          → RPM database

Repository File:
[reponame]
name=Description
baseurl=http://example.com/repo
enabled=1
gpgcheck=1

Commands:
├── yum clean metadata     → Clear cache
├── yum repolist           → List enabled repos
└── man yum.conf           → View all options
```
