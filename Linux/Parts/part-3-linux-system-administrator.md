# Part 3: Becoming a Linux System Administrator

> **Based on:** Linux Bible, 10th Edition - Chapters 8-12

This part transitions from user-level tasks to **system administration**, covering installation, software management, user accounts, and disk management.

---

## 📑 Chapter Overview

| Chapter | Title | Focus |
|---------|-------|-------|
| 8 | Learning System Administration | Root access, admin tools, hardware |
| 9 | Installing Linux | Installation methods, partitioning |
| 10 | Getting and Managing Software | Package managers, repositories |
| 11 | Managing User Accounts | User/group management, permissions |
| 12 | Managing Disks and Filesystems | Partitions, filesystems, LVM |

---

## 📝 Chapter 8: Learning System Administration

### Section 1: System Administration Fundamentals

| # | Topic | Notes File |
|---|-------|------------|
| 1 | Admin Basics | [01-system-admin-basics.md](../Notes/part-3/01-system-admin-basics.md) |
| 2 | Gaining Root Access | [02-gaining-root-access.md](../Notes/part-3/02-gaining-root-access.md) |
| 3 | Commands & Files | [03-admin-commands-files.md](../Notes/part-3/03-admin-commands-files.md) |
| 4 | Hardware Management | [04-hardware-management.md](../Notes/part-3/04-hardware-management.md) |

### Section 2: Installing Linux

| # | Topic | Notes File |
|---|-------|------------|
| 5 | Installation Methods | [05-installation-methods.md](../Notes/part-3/05-installation-methods.md) |
| 6 | Installation Procedures | [06-installation-procedures.md](../Notes/part-3/06-installation-procedures.md) |
| 7 | Disk Partitioning | [07-disk-partitioning.md](../Notes/part-3/07-disk-partitioning.md) |
| 8 | GRUB Boot Loader | [08-grub-boot-loader.md](../Notes/part-3/08-grub-boot-loader.md) |

### Section 3: Managing Software

| # | Topic | Notes File |
|---|-------|------------|
| 9 | Packaging Basics | [09-software-packaging-basics.md](../Notes/part-3/09-software-packaging-basics.md) |
| 10 | Graphical Management | [10-managing-software-graphically.md](../Notes/part-3/10-managing-software-graphically.md) |
| 11 | YUM/DNF Repositories | [11-yum-dnf-repositories.md](../Notes/part-3/11-yum-dnf-repositories.md) |
| 12 | YUM Package Management | [12-yum-package-management.md](../Notes/part-3/12-yum-package-management.md) |
| 13 | RPM Command | [13-rpm-command.md](../Notes/part-3/13-rpm-command.md) |

### Section 4: User and Group Management

| # | Topic | Notes File |
|---|-------|------------|
| 14 | Creating Users | [14-creating-user-accounts.md](../Notes/part-3/14-creating-user-accounts.md) |
| 15 | Modifying/Deleting Users | [15-modifying-deleting-users.md](../Notes/part-3/15-modifying-deleting-users.md) |
| 16 | Managing Groups | [16-managing-groups.md](../Notes/part-3/16-managing-groups.md) |
| 17 | Advanced Permissions | [17-advanced-permissions.md](../Notes/part-3/17-advanced-permissions.md) |

### Section 5: Managing Disks and Filesystems

| # | Topic | Notes File |
|---|-------|------------|
| 18 | Disk Storage Basics | [18-disk-storage-basics.md](../Notes/part-3/18-disk-storage-basics.md) |
| 19 | Disk Partitioning | [19-disk-partitioning.md](../Notes/part-3/19-disk-partitioning.md) |
| 20 | LVM Logical Volumes | [20-lvm-logical-volumes.md](../Notes/part-3/20-lvm-logical-volumes.md) |
| 21 | Filesystems and Mounting | [21-filesystems-mounting.md](../Notes/part-3/21-filesystems-mounting.md) |
| 22 | Creating Filesystems | [22-creating-filesystems.md](../Notes/part-3/22-creating-filesystems.md) |

---

## 🎯 Learning Objectives

By the end of this part, you should be able to:
- **Gain Root Privilege:** Use `su`, `sudo`, and understand when root access is needed.
- **Use Admin Tools:** Navigate Cockpit, understand config files and logs.
- **Manage Hardware:** Check hardware with `lspci`, `lsusb`, manage kernel modules.
- **Install Linux:** Choose installation methods, perform Live/DVD installations, use boot options.
- **Partition Disks:** Understand partition types (Linux, LVM, RAID, swap), plan partition layout, use parted/fdisk.
- **Configure Boot Loader:** Understand GRUB 2, modify boot entries, recover systems.
- **Manage Software:** Use YUM/DNF to search, install, update, and remove packages; understand RPM packaging.
- **Manage Users:** Create users (useradd/Cockpit), modify (usermod), delete (userdel); manage groups (groupadd); use ACLs.
- **Manage Storage:** Partition disks with parted/fdisk; create LVM volumes; format filesystems with mkfs; mount/unmount filesystems.

## ⌨️ Key Commands

```bash
# Root Access
su, sudo, visudo

# System Info
dmesg, lspci, lsusb, lscpu, journalctl

# Modules
lsmod, modprobe, rmmod, modinfo

# Installation & Partitioning
fdisk, parted, mkfs, lsblk, blkid

# Boot Loader
grub2-mkconfig, grub2-install, grub2-set-default

# Package Management
dnf, apt, rpm, dpkg

# User Management
useradd, usermod, userdel, passwd, groupadd

# Disk Management
mount, umount, lvm, pvcreate, vgcreate, lvcreate
```

---

## 📚 Additional Resources

- **Man Pages:** `man sudo`, `man journalctl`, `man lsmod`
- **Cockpit:** https://cockpit-project.org
- **RHEL Documentation:** https://access.redhat.com/documentation

---

## 📝 Quiz

Test your knowledge: [Part 3 Quiz](../Quiz/Part-3/part-3-quiz.md)
