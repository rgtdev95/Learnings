# RPM Command

## Understanding rpm

The `rpm` command is the **low-level** tool for managing RPM packages.

**Use when:**
*   Working with local .rpm files
*   Querying installed packages
*   Verifying package integrity
*   Need detailed package information

**Don't use for:**
*   Installing from repositories (use `dnf`)
*   Resolving dependencies automatically (use `dnf`)

---

## Installing Packages with rpm

### Basic Install

```bash
sudo rpm -i zsh-5.5.1-6.el8.x86_64.rpm
```

**Limitations:**
*   Must provide full filename
*   Doesn't resolve dependencies
*   Fails if dependencies missing

### Upgrade Package

```bash
sudo rpm -Uhv zsh-5.5.1-6.el8.x86_64.rpm
```

**Options:**
*   `-U` = Upgrade (install if not present)
*   `-h` = Show hash marks (#) during install
*   `-v` = Verbose output

**Upgrade vs Install:**
*   `-i` = Fails if package already installed
*   `-U` = Installs even if already present
*   `-U` = Preferred for general use

### Freshen Package

```bash
sudo rpm -Fhv *.rpm
```

**Freshen (-F):**
*   Only upgrades already-installed packages
*   Skips packages not currently installed
*   Useful for updating a directory of RPMs

### Special Install Options

**Reinstall existing package:**
```bash
sudo rpm -Uhv --replacepkgs emacs-26.1-5.el8.x86_64.rpm
```

**Downgrade to older version:**
```bash
sudo rpm -Uhv --oldpackage zsh-5.0.2-25.el7.x86_64.rpm
```

---

## Removing Packages

### Basic Removal

```bash
sudo rpm -e emacs
```

**Notes:**
*   Only need package base name (not full filename)
*   Fails if other packages depend on it
*   Doesn't remove dependencies

---

## Querying Packages

### Query Installed Packages

**List all installed packages:**
```bash
rpm -qa
```

**Query specific package:**
```bash
rpm -q firefox
```

**Package information:**
```bash
rpm -qi zsh
```

**Output includes:**
*   Name, version, release
*   Install date
*   Size
*   License
*   Summary and description
*   URL, packager

### List Package Files

**All files in package:**
```bash
rpm -ql zsh
```

**Documentation files:**
```bash
rpm -qd zsh
```

**Configuration files:**
```bash
rpm -qc zsh
```

---

## Querying Package Details

### Dependencies

**What package requires:**
```bash
rpm -q --requires emacs-common
```

**What package provides:**
```bash
rpm -q --provides emacs-common
```

### Scripts

**View install/uninstall scripts:**
```bash
rpm -q --scripts httpd
```

**Shows:**
*   Pre-install scripts
*   Post-install scripts
*   Pre-uninstall scripts
*   Post-uninstall scripts

### Change Log

```bash
rpm -q --changelog httpd | less
```

**Shows:**
*   Version history
*   Bug fixes
*   Feature additions
*   Maintainer notes

### Query Tags

**List available query tags:**
```bash
rpm --querytags
```

**Custom query format:**
```bash
rpm -q binutils --queryformat \
"Package: %{NAME} Release: %{RELEASE}\n"
```

**Output:**
```
Package: binutils Release: 29.fc30
```

---

## Querying RPM Files (Not Installed)

Add `-p` to query a local .rpm file instead of installed package.

**Package information:**
```bash
rpm -qip zsh-5.7.1-1.fc30.x86_64.rpm
```

**List files:**
```bash
rpm -qlp zsh-5.7.1-1.fc30.x86_64.rpm
```

**Documentation:**
```bash
rpm -qdp zsh-5.7.1-1.fc30.x86_64.rpm
```

**Config files:**
```bash
rpm -qcp zsh-5.7.1-1.fc30.x86_64.rpm
```

---

## Verifying Packages

### Basic Verification

```bash
rpm -V zsh
```

**No output** = All files intact

**Output indicates changes:**
```
S.5....T.  /bin/zsh
missing c /etc/zshrc
```

### Verification Codes

Each position shows a test result:

| Position | Test | Code Letters |
|----------|------|--------------|
| 1 | File Size | `S` = Size changed |
| 2 | Mode/Permissions | `M` = Mode changed |
| 3 | MD5 Checksum | `5` = MD5 changed |
| 4 | Device | `D` = Device mismatch |
| 5 | Symlink | `L` = Link path changed |
| 6 | User | `U` = User ownership changed |
| 7 | Group | `G` = Group ownership changed |
| 8 | Modification Time | `T` = mTime changed |
| 9 | Capabilities | `P` = Capabilities changed |

**File type flags:**
*   `c` = Configuration file
*   `d` = Documentation file

### Example Verification

**Install and modify package:**
```bash
sudo rpm -i zsh-5.7.1-1.fc30.x86_64.rpm
echo "modified" > /bin/zsh
rm /etc/zshrc
```

**Verify:**
```bash
rpm -V zsh
```

**Output:**
```
missing   c /etc/zshrc
S.5....T.   /bin/zsh
```

**Interpretation:**
*   `/etc/zshrc` is missing (config file)
*   `/bin/zsh` has changed size, MD5 sum, and mTime

### Restore Package

```bash
sudo rpm -i --replacepkgs zsh-5.7.1-1.fc30.x86_64.rpm
rpm -V zsh
```

**No output** = Package restored to original state

---

## Finding Which Package Owns a File

```bash
rpm -qf /bin/ls
```

**Output:**
```
coreutils-8.30-6.el8.x86_64
```

---

## RPM Database Maintenance

### Database Location

`/var/lib/rpm/`

### Rebuild Database

If database corrupted:

```bash
sudo rpm --rebuilddb
```

### Backup Best Practice

1. Back up `/var/lib/rpm/`
2. Store on read-only media (CD/USB)
3. Use for verification if system compromised

---

## Common RPM Workflows

### Examine Unknown RPM

```bash
# Get info
rpm -qip mysterious.rpm

# List files
rpm -qlp mysterious.rpm

# Check dependencies
rpm -qRp mysterious.rpm
```

### Install Local RPM

```bash
# Install
sudo rpm -Uvh package.rpm

# Verify installation
rpm -V package-name
```

### Verify System Integrity

```bash
# Verify all packages
rpm -Va | less

# Verify specific package
rpm -V httpd
```

### Find Modified Files

```bash
# Check all packages, show only changes
rpm -Va | grep '^..5'
```

---

## Review Questions

1. What does `rpm -Uhv` do?

2. What's the difference between `-i` and `-U` options?

3. Which option queries a local .rpm file instead of installed packages?

4. True or False: RPM automatically resolves dependencies.

5. What does `rpm -V package` do?

6. In verification output, what does `S.5....T.` mean?

7. How do you find which package owns `/bin/bash`?

8. Where is the RPM database stored?

---

## Quick Reference

```
Installing:
├── rpm -i PKG.rpm         → Install
├── rpm -Uhv PKG.rpm       → Upgrade (verbose + progress)
├── rpm -Fhv *.rpm         → Freshen (update installed only)
└── rpm -e PKG             → Erase/remove

Querying Installed:
├── rpm -qa                → List all
├── rpm -qi PKG            → Package info
├── rpm -ql PKG            → List files
├── rpm -qd PKG            → List docs
├── rpm -qc PKG            → List configs
├── rpm -qf FILE           → Which pkg owns file
└── rpm -q --changelog PKG → View changes

Querying RPM File:
├── rpm -qip PKG.rpm       → File info
├── rpm -qlp PKG.rpm       → List files in RPM
└── rpm -qRp PKG.rpm       → Show requirements

Verifying:
├── rpm -V PKG             → Verify package
└── rpm -Va                → Verify all packages

Database:
├── /var/lib/rpm/          → Database location
└── rpm --rebuilddb        → Rebuild database
```
