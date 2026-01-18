# Software Packaging Basics

## Key Concepts

| Term | Definition |
|------|------------|
| **Package** | Collection of files + metadata for a software feature |
| **RPM** | RPM Package Manager (Red Hat-based distros) |
| **DEB** | Debian package format (Debian-based distros) |
| **Repository** | Collection of packages available for installation |
| **Metadata** | Information about package (dependencies, version, etc.) |

---

## Linux Package Formats

### RPM (.rpm) - Red Hat Package Manager

**Used by:** RHEL, Fedora, CentOS, Oracle Linux, SUSE

**Tools:**
*   `rpm` - Low-level package tool
*   `yum` - YUM Package Manager (older)
*   `dnf` - Dandified YUM (newer, RHEL 8+)

### DEB (.deb) - Debian Package

**Used by:** Debian, Ubuntu, Linux Mint, KNOPPIX

**Tools:**
*   `dpkg` - Low-level package tool
*   `apt` / `apt-get` - Advanced Package Tool
*   `aptitude` - Menu-based package manager

---

## Why Packages Instead of Tarballs?

**Old Method (Tarball):**
*   Just extract files to system
*   No dependency tracking
*   Hard to remove/update
*   No version information

**Modern Packaging Advantages:**

| Feature | Benefit |
|---------|---------|
| **Dependency Management** | Auto-install required software |
| **Version Tracking** | Know what's installed |
| **Easy Updates** | Replace old versions cleanly |
| **Easy Removal** | Remove all components |
| **Validation** | Verify package integrity |

---

## RPM Package Structure

### Package Naming Convention

**Format:** `name-version-release-architecture.rpm`

**Example:** `firefox-67.0-4.fc30.x86_64.rpm`

| Component | Meaning |
|-----------|---------|
| `firefox` | Package base name |
| `67.0` | Version (from upstream project) |
| `4` | Release (packaging revision) |
| `fc30` | Distribution (Fedora 30) |
| `x86_64` | Architecture (64-bit Intel/AMD) |

### Common Architectures

*   `x86_64` - 64-bit Intel/AMD
*   `i686` or `i386` - 32-bit Intel
*   `noarch` - Architecture-independent (scripts, docs)
*   `src` - Source RPM (not binary)

### What's Inside an RPM?

**Files:**
*   Commands (executables)
*   Configuration files
*   Documentation
*   Libraries
*   Man pages

**Metadata:**
*   Package name, version, release
*   Dependencies (required packages)
*   Description and summary
*   License information
*   Install/uninstall scripts
*   File checksums (md5sums)
*   Build information

---

## RPM Package Lifecycle

### 1. Creation (Upstream → Distribution)

```
Upstream Project
    ↓
Source Code
    ↓
Distribution Packager builds RPM
    ↓
RPM signed with GPG key
    ↓
RPM placed in repository
```

### 2. Installation

```
Repository (web/FTP/NFS/DVD)
    ↓
Download RPM + dependencies
    ↓
Verify signature
    ↓
Run pre-install scripts
    ↓
Copy files to filesystem
    ↓
Run post-install scripts
    ↓
Update local RPM database
```

### 3. Database Storage

**Location:** `/var/lib/rpm/`

**Contains:**
*   All installed packages
*   File checksums
*   Package relationships
*   Installation dates

---

## DEB Package Overview

**Example:** `firefox_67.0-1ubuntu1_amd64.deb`

**Structure:** ar archive containing:
*   `control.tar.gz` - Metadata
*   `data.tar.xz` - Actual files
*   `debian-binary` - Version info

**Common Commands:**

```bash
# Search for package
sudo apt-cache search vsftpd

# Show package info
sudo apt-cache show vsftpd

# Install package
sudo apt-get install vsftpd

# Update all packages
sudo apt-get upgrade

# List installed packages
sudo apt-cache pkgnames
```

---

## RPM vs DEB: Key Differences

| Feature | RPM | DEB |
|---------|-----|-----|
| **Low-level tool** | `rpm` | `dpkg` |
| **High-level tool** | `yum` / `dnf` | `apt` / `apt-get` |
| **File extension** | `.rpm` | `.deb` |
| **Archive format** | cpio | ar |
| **Main distros** | RHEL, Fedora | Debian, Ubuntu |

**Similarities:**
*   Both handle dependencies
*   Both track installed packages
*   Both verify package integrity
*   Both support repositories

---

## Package Management Advantages

### Dependency Resolution

**Problem:** Package A needs library B

**Solution:**
*   RPM: `yum` / `dnf` auto-installs library B
*   DEB: `apt` auto-installs library B

### Version Control

Know exactly what's installed:
```bash
rpm -qa                  # List all RPMs
dpkg -l                  # List all DEBs
```

### Security

*   Packages signed with GPG keys
*   Verify packages haven't been tampered with
*   Track which packages need security updates

---

## Review Questions

1. What does RPM stand for?

2. What are the three main components of an RPM package name?

3. Which command-line tool is used for low-level RPM operations?

4. Where is the local RPM database stored?

5. True or False: DEB packages use the same format as RPM packages.

6. What are two advantages of packages over tarballs?

7. Which architecture designation indicates a package works on any CPU type?

8. What does `noarch` mean in a package name?

---

## Quick Reference

```
RPM Package Name:
firefox-67.0-4.fc30.x86_64.rpm
├── firefox     → Base name
├── 67.0        → Version
├── 4           → Release
├── fc30        → Distribution
└── x86_64      → Architecture

Package Formats:
├── RPM  → RHEL, Fedora, CentOS
└── DEB  → Debian, Ubuntu, Mint

RPM Database:
└── /var/lib/rpm/  → Installed package info
```
