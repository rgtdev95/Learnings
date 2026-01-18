# YUM Package Management

## Common YUM/DNF Commands

All examples work with both `yum` and `dnf`.

---

## Searching for Packages

### Search by Name or Description

```bash
dnf search editor
```

**Output:**
```
emacs.x86_64 : GNU Emacs text editor
vim.x86_64 : Enhanced version of the vi editor
ed.x86_64 : The GNU line editor
```

### Show Package Information

```bash
dnf info emacs
```

**Shows:**
*   Name, version, release
*   Architecture
*   Size
*   Summary and description
*   Repository source
*   License
*   URL

### Find Package Providing a File

```bash
dnf provides /usr/bin/dvdrecord
```

**Output:**
```
wodim-1.1.11-41.fc30.x86_64 : CD/DVD recording program
Filename: /usr/bin/dvdrecord
```

---

## Listing Packages

### List Specific Package

```bash
dnf list emacs
```

### List Available Packages

```bash
dnf list available
```

### List Installed Packages

```bash
dnf list installed
```

### List All Packages

```bash
dnf list all
```

---

## Installing and Removing Packages

### Install a Package

```bash
sudo dnf install emacs
```

**Process:**
1. Finds package in repository
2. Calculates dependencies
3. Shows download size and install size
4. Asks for confirmation (y/N)
5. Downloads all packages
6. Installs packages

**Auto-confirm:**
```bash
sudo dnf install -y emacs
```

### Reinstall a Package

If components were deleted:

```bash
sudo dnf reinstall zsh
```

### Remove a Package

```bash
sudo dnf remove emacs
```

**Also removes:**
*   Dependencies no longer needed by other packages

**Shows:**
*   Space to be freed

---

## Viewing Dependencies

### List Dependencies

```bash
dnf deplist emacs
```

**Shows:**
```
package: emacs-1:26.1-8.fc30.x86_64
  dependency: /bin/sh
    provider: bash-5.0.7-1.fc30.x86_64
  dependency: emacs-common
    provider: emacs-common-1:26.1-8.fc30.x86_64
```

---

## Updating Packages

### Check for Updates

```bash
dnf check-update
```

**Lists:**
*   Packages with updates available
*   New version numbers
*   Repository source

### Update All Packages

```bash
sudo dnf update
```

**Process:**
1. Downloads all available updates
2. Shows package count and size
3. Asks for confirmation
4. Installs updates

### Update Specific Package

```bash
sudo dnf update cups
```

---

## Working with Package Groups

Package groups = Related packages bundled together

### List Package Groups

```bash
dnf grouplist
```

**Shows:**
*   Available Environment Groups (full desktops)
*   Installed Groups
*   Available Groups

### Show Group Information

```bash
dnf groupinfo LXDE
```

**Shows:**
*   Group description
*   Mandatory packages
*   Default packages
*   Optional packages

### Install a Group

```bash
sudo dnf groupinstall LXDE
```

**Installs:**
*   All mandatory packages
*   All default packages
*   (Optional packages not installed by default)

### Remove a Group

```bash
sudo dnf groupremove LXDE
```

---

## Using History

YUM/DNF tracks all transactions.

### View History

```bash
dnf history
```

**Shows:**
```
ID | Command line        | Date and time
---+--------------------+------------------
12 | install emacs      | 2019-06-22 11:14
11 | update             | 2019-06-20 09:30
```

### View Transaction Details

```bash
dnf history info 12
```

### Undo a Transaction

```bash
sudo dnf history undo 12
```

**Reverses:**
*   All installs become uninstalls
*   All updates revert to previous versions

---

## Maintenance Commands

### Clean Package Cache

**Delete  downloaded packages:**
```bash
sudo dnf clean packages
```

**Delete metadata:**
```bash
sudo dnf clean metadata
```

**Delete everything:**
```bash
sudo dnf clean all
```

### Check for Problems

```bash
dnf check
```

**Checks:**
*   RPM database consistency
*   Dependency issues

### Rebuild RPM Database

If database is corrupted:

```bash
sudo rpm --rebuilddb
```

---

## Downloading Without Installing

### Download Package File

```bash
dnf download firefox
```

**Result:**
*   Package saved to current directory
*   Not installed
*   Can examine with `rpm -qp`

---

## Advanced Options

### Install from Local File

```bash
sudo dnf install ./mypackage.rpm
```

### Skip Broken Dependencies

```bash
sudo dnf install --skip-broken package
```

### Assume Yes to All Prompts

```bash
sudo dnf install -y package
```

### Download Only (Don't Install)

```bash
sudo dnf install --downloadonly package
```

---

## Common Workflows

### Install New Software

```bash
# Search
dnf search apache

# Get info
dnf info httpd

# Install
sudo dnf install httpd
```

### Update System

```bash
# Check for updates
dnf check-update

# Apply all updates
sudo dnf update

# Reboot if kernel updated
sudo reboot
```

### Remove Unwanted Software

```bash
# List what's installed
dnf list installed | grep game

# Remove
sudo dnf remove game-name
```

### Fix Broken Package

```bash
# Remove corrupted package
sudo dnf remove package

# Reinstall clean copy
sudo dnf install package
```

---

## Review Questions

1. What command searches for packages containing "editor"?

2. How do you find which package provides `/usr/bin/ls`?

3. What's the difference between `dnf install` and `dnf reinstall`?

4. Which command shows dependencies of a package?

5. True or False: `dnf update` updates only one package at a time.

6. What does `dnf groupinstall` do?

7. How do you undo a previous dnf transaction?

8. Which command downloads a package without installing it?

---

## Quick Reference

```
Searching:
├── dnf search TERM        → Find packages
├── dnf info PKG           → Package details
├── dnf provides FILE      → Which package has file
└── dnf list installed     → List installed

Installing/Removing:
├── dnf install PKG        → Install
├── dnf reinstall PKG      → Reinstall
├── dnf remove PKG         → Remove
└── dnf update             → Update all

Groups:
├── dnf grouplist          → List groups
├── dnf groupinfo GROUP    → Group details
├── dnf groupinstall GROUP → Install group
└── dnf groupremove GROUP  → Remove group

Maintenance:
├── dnf clean all          → Clear cache
├── dnf check              → Check for problems
├── dnf history            → View transactions
└── dnf history undo ID    →  Undo transaction
```
