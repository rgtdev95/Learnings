# Software Package Troubleshooting

## Package Management Overview

**RPM-based (RHEL/Fedora):**
*   `dnf` - Modern package manager (RHEL 8+, Fedora 22+)
*   `yum` - Legacy package manager (RHEL 7 and earlier)
*   `rpm` - Low-level package tool

**DEB-based (Ubuntu/Debian):**
*   `apt-get` - Package manager
*   `dpkg` - Low-level package tool

---

## Dependency Problems

**Common causes:**
*   Using third-party repositories
*   Mixing repositories
*   Outdated system
*   Development/unstable repositories

**Best practices:**

**Use stable repositories:**
*   Stick to official distribution repos
*   Avoid third-party when possible

**Update regularly:**
```bash
dnf update  # Daily or weekly
```

**Upgrade system annually:**
```bash
# Fedora upgrade process
dnf upgrade --refresh -y
dnf install dnf-plugin-system-upgrade -y
dnf system-upgrade download --releasever=31
dnf system-upgrade reboot
```

---

## Resolving Dependencies

**Exclude problematic packages:**
```bash
yum -y --exclude=somepackage update
```

**Check for kernel-related issues:**
*   Third-party drivers may break with new kernel
*   Boot previous kernel from GRUB
*   Wait for updated drivers
*   Use rpmfusion.org for automatic driver updates

**Reinstall to fix:**
*   Sometimes reinstalling fixes dependency issues
*   Fresh install may be needed for major problems

---

## RPM Database Issues

**Symptoms:**
*   rpm/yum commands hang
*   "rpmdb open fails" errors
*   Database corruption

**Check for problems:**
```bash
yum check
```

**Example error:**
```
error: db4 error(11) from dbenv->open
error: cannot open Packages index
CRITICAL:yum.main: Error: rpmdb open fails
```

**Fix corrupted database:**
```bash
# Backup first
cp -r /var/lib/rpm /tmp

# Remove and rebuild
cd /var/lib/rpm
rm __db*
rpm --initdb
```

---

## DNF/Yum Cache Issues

**Cache location:**
*   DNF: `/var/cache/dnf`
*   Yum: `/var/cache/yum`

**When to clean:**

**Obsolete metadata:**
*   Metadata older than 48 hours
*   Repository changes not reflected

**Running out of disk space:**
*   Cache can grow to gigabytes
*   Especially with `keepcache=1`

**Clean cache:**
```bash
# Clean all cache
yum clean all

# Clean only metadata
dnf clean metadata

# Update metadata
dnf makecache
```

---

## Automatic Updates

**Using cron:**

**Create crontab entry:**
```bash
crontab -e
```

**Add update job:**
```
# min hour day/month month day/week command
59   23   *         *     *        dnf -y update | mail root@localhost
```

**Cron field examples:**
*   `1,5,17` - Minutes 1, 5, and 17
*   `*/3` - Every 3 hours
*   `1-3` - January through March

**AutoUpdates (Fedora 22+, RHEL 8+):**
*   Automatically downloads updates
*   Can be configured to install
*   See: https://fedoraproject.org/wiki/AutoUpdates

---

## Package Installation Issues

**Missing dependencies:**
```bash
# Install with dependencies
dnf install package-name
```

**Conflicting packages:**
```bash
# Remove conflicting package first
dnf remove conflicting-package
dnf install new-package
```

**Package not found:**
```bash
# Search for package
dnf search package-name

# Check enabled repositories
dnf repolist

# Enable additional repo
dnf config-manager --enable repo-name
```

---

## Repository Management

**List repositories:**
```bash
dnf repolist
dnf repolist all  # Including disabled
```

**Enable/disable repository:**
```bash
dnf config-manager --enable repo-name
dnf config-manager --disable repo-name
```

**Add repository:**
```bash
# Add repo file to /etc/yum.repos.d/
vi /etc/yum.repos.d/custom.repo
```

**Repository file example:**
```
[custom-repo]
name=Custom Repository
baseurl=http://example.com/repo
enabled=1
gpgcheck=1
gpgkey=http://example.com/RPM-GPG-KEY
```

---

## Review Questions

1. What command updates all packages in Fedora/RHEL?

2. How do you exclude a package from update?

3. Where is the RPM database stored?

4. What command rebuilds the RPM database?

5. How do you clean the DNF cache?

6. True or False: yum update in RHEL upgrades to the next release.

7. What's the difference between `apt-get update` and `apt-get upgrade`?

8. How do you check for RPM database corruption?

---

## Quick Reference

```
Package Management:
├── dnf install package     → Install package
├── dnf update             → Update all packages
├── dnf remove package     → Remove package
├── dnf search keyword     → Search packages
└── dnf info package       → Package information

Dependency Issues:
├── Use stable repositories
├── Update regularly
├── Exclude: yum -y --exclude=pkg update
└── Kernel issues: Boot previous kernel

RPM Database:
├── Location: /var/lib/rpm
├── Check: yum check
├── Backup: cp -r /var/lib/rpm /tmp
├── Fix: rm __db* && rpm --initdb
└── Files: __db*

Cache Management:
├── Location: /var/cache/dnf
├── Clean all: yum clean all
├── Clean metadata: dnf clean metadata
└── Update metadata: dnf makecache

Automatic Updates:
├── Cron: crontab -e
├── Entry: 59 23 * * * dnf -y update
└── AutoUpdates: Fedora 22+, RHEL 8+

Repository Management:
├── List: dnf repolist
├── Enable: dnf config-manager --enable repo
├── Disable: dnf config-manager --disable repo
└── Config: /etc/yum.repos.d/

System Upgrade (Fedora):
1. dnf upgrade --refresh -y
2. dnf install dnf-plugin-system-upgrade -y
3. dnf system-upgrade download --releasever=31
4. dnf system-upgrade reboot
```
