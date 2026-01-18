# Part 3: Linux System Administrator - Quiz & Flashcards

> **How to use:** Click on "Answer" to reveal the answer. Test yourself before revealing!

---

## Topic 1: System Administration Basics

**Q1:** What is the User ID (UID) of the root user?
<details>
<summary>Answer</summary>

**0** (zero)

</details>

---

**Q2:** Where is the root user's home directory located?
<details>
<summary>Answer</summary>

`/root`

</details>

---

**Q3:** Which user typically runs the Apache web server on RHEL/Fedora systems?
<details>
<summary>Answer</summary>

`apache` (on Ubuntu, it's `www-data`)

</details>

---

## Topic 2: Gaining Root Access

**Q4:** What is the difference between `su` and `su -`?
<details>
<summary>Answer</summary>

- `su`: Switches to root but keeps current environment
- `su -`: Switches to root AND loads root's full environment (recommended)

</details>

---

**Q5:** Which command do you use to safely edit the `/etc/sudoers` file?
<details>
<summary>Answer</summary>

`visudo`

(It locks the file and checks syntax)

</details>

---

**Q6:** On what port does Cockpit run by default?
<details>
<summary>Answer</summary>

**Port 9090** (HTTPS)

</details>

---

**Q7:** When using `sudo`, whose password do you enter?
<details>
<summary>Answer</summary>

Your **own** user password (not the root password)

</details>

---

## Topic 3: Administrative Files

**Q8:** Where are most system-wide configuration files stored?
<details>
<summary>Answer</summary>

`/etc/`

</details>

---

**Q9:** Which file contains user account information?
<details>
<summary>Answer</summary>

`/etc/passwd`

</details>

---

**Q10:** Which command is used to view the systemd journal?
<details>
<summary>Answer</summary>

`journalctl`

</details>

---

**Q11:** How do you follow journal messages in real time?
<details>
<summary>Answer</summary>

`journalctl -f`

</details>

---

**Q12:** Where does rsyslogd typically store log files?
<details>
<summary>Answer</summary>

`/var/log/`

Common examples: `/var/log/messages`, `/var/log/secure`

</details>

---

## Topic 4: Hardware Management

**Q13:** Which command shows kernel boot messages?
<details>
<summary>Answer</summary>

`dmesg`

</details>

---

**Q14:** What command lists all PCI devices on your system?
<details>
<summary>Answer</summary>

`lspci`

</details>

---

**Q15:** Which command displays connected USB devices?
<details>
<summary>Answer</summary>

`lsusb`

</details>

---

**Q16:** Which command lists currently loaded kernel modules?
<details>
<summary>Answer</summary>

`lsmod`

</details>

---

**Q17:** What does the `modprobe` command do?
<details>
<summary>Answer</summary>

Loads a kernel module (driver) into the running kernel

</details>

---

**Q18:** Where are removable USB drives typically auto-mounted in modern Linux?
<details>
<summary>Answer</summary>

`/run/media/username/`

</details>

---

## Bonus: True/False

**Q19-22:** True or False?

<details>
<summary>Questions</summary>

19. The root user has UID 0.
20. `sudo` requires the root password.
21. Configuration files in Linux are usually plain text.
22. You can unload any kernel module with `rmmod`.

</details>

<details>
<summary>Answers</summary>

19. **TRUE**
20. **FALSE** - sudo uses your own password
21. **TRUE** - Most configs are plain text
22. **FALSE** - Cannot unload built-in or in-use modules

</details>

---

## Topic 5: Installation Methods

**Q23:** Which installation method copies the entire Live system to disk?
<details>
<summary>Answer</summary>

**Live Media** installation

</details>

---

**Q24:** What are the three main components of enterprise Linux installation?
<details>
<summary>Answer</summary>

1. **PXE Boot Server** (network boot)
2. **Installation Server** (hosts packages)
3. **Kickstart Files** (automated answers)

</details>

---

**Q25:** What does PXE stand for?
<details>
<summary>Answer</summary>

**Preboot eXecution Environment**

</details>

---

**Q26:** Which CPU feature is required for KVM virtualization?
<details>
<summary>Answer</summary>

**AMD-V** or **Intel-VT** (virtualization support)

</details>

---

## Topic 6: Installation Procedures

**Q27:** What command updates all packages on a Fedora system after installation?
<details>
<summary>Answer</summary>

`sudo dnf update`

</details>

---

**Q28:** Which boot option forces a text-mode installation?
<details>
<summary>Answer</summary>

`text`

</details>

---

**Q29:** How do you specify a kickstart file location during boot?
<details>
<summary>Answer</summary>

`ks=URL`

Examples:
- `ks=cdrom:/ks.cfg`
- `ks=http://server/ks.cfg`

</details>

---

**Q30:** For dual-boot systems, which operating system should be installed first?
<details>
<summary>Answer</summary>

**Windows** should be installed first, then Linux.

(Windows can damage Linux partitions; GRUB can boot both)

</details>

---

## Topic 7: Disk Partitioning

**Q31:** What is the default filesystem type for RHEL 8?
<details>
<summary>Answer</summary>

**xfs**

</details>

---

**Q32:** What does LVM stand for?
<details>
<summary>Answer</summary>

**Logical Volume Manager**

</details>

---

**Q33:** How many primary partitions can a GPT partition table have?
<details>
<summary>Answer</summary>

**128** primary partitions

(MBR can only have 4 primary partitions)

</details>

---

**Q34:** Which directory is commonly placed on a separate partition to prevent boot issues?
<details>
<summary>Answer</summary>

`/boot`

(Usually 1024 MiB, ensures BIOS can access boot files)

</details>

---

**Q35:** What Linux tool can resize Windows NTFS partitions?
<details>
<summary>Answer</summary>

**GParted** (GNOME Partition Editor)

</details>

---

## Topic 8: GRUB Boot Loader

**Q36:** What does GRUB stand for?
<details>
<summary>Answer</summary>

**GRand Unified Bootloader**

</details>

---

**Q37:** Which GRUB 2 config file should you NOT edit directly?
<details>
<summary>Answer</summary>

`/boot/grub2/grub.cfg`

(Auto-generated - edit `/etc/default/grub` instead)

</details>

---

**Q38:** What command regenerates the GRUB configuration?
<details>
<summary>Answer</summary>

`sudo grub2-mkconfig -o /boot/grub2/grub.cfg`

</details>

---

**Q39:** What kernel option boots Linux into rescue mode?
<details>
<summary>Answer</summary>

`rd.break` or `systemd.unit=rescue.target`

</details>

---

## Bonus: Installation True/False

**Q40-43:** True or False?

<details>
<summary>Questions</summary>

40. Kickstart files can contain custom scripts.
41. GRUB 2 uses UUIDs for device identification.
42. An MBR partition table supports 128 primary partitions.
43. You should always backup data before partitioning.

</details>

<details>
<summary>Answers</summary>

40. **TRUE** - Pre and post-install scripts
41. **TRUE** - More reliable than device names
42. **FALSE** - MBR supports only 4 primary (GPT supports 128)
43. **TRUE** - Partitioning can destroy data!

</details>

---

## Topic 9: Software Packaging

**Q44:** What does RPM stand for?
<details>
<summary>Answer</summary>

**RPM Package Manager** (originally Red Hat Package Manager)

</details>

---

**Q45:** What are the four components of an RPM package name?
<details>
<summary>Answer</summary>

`name-version-release.architecture`

Example: `firefox-67.0-4.fc30.x86_64`

</details>

---

**Q46:** Where is the local RPM database stored?
<details>
<summary>Answer</summary>

`/var/lib/rpm/`

</details>

---

## Topic 10: YUM/DNF

**Q47:** What does YUM stand for?
<details>
<summary>Answer</summary>

**YellowDog Updater Modified**

</details>

---

**Q48:** Where are YUM repository configuration files stored?
<details>
<summary>Answer</summary>

`/etc/yum.repos.d/` (*.repo files)

</details>

---

**Q49:** What command searches for packages containing "apache"?
<details>
<summary>Answer</summary>

`dnf search apache`

</details>

---

**Q50:** How do you install a package with DNF?
<details>
<summary>Answer</summary>

`sudo dnf install package-name`

</details>

---

**Q51:** What does `dnf update` do?
<details>
<summary>Answer</summary>

Updates all installed packages that have newer versions available

</details>

---

**Q52:** How do you remove a package and its unused dependencies?
<details>
<summary>Answer</summary>

`sudo dnf remove package-name`

</details>

---

**Q53:** What command shows which package provides a specific file?
<details>
<summary>Answer</summary>

`dnf provides /path/to/file`

Example: `dnf provides /usr/bin/ls`

</details>

---

## Topic 11: RPM Command

**Q54:** What's the difference between `rpm -i` and `rpm -U`?
<details>
<summary>Answer</summary>

- `rpm -i`: Install only (fails if already installed)
- `rpm -U`: Upgrade (installs even if not present)

</details>

---

**Q55:** Which rpm option queries a local .rpm file?
<details>
<summary>Answer</summary>

`-p` (package file)

Example: `rpm -qip package.rpm`

</details>

---

**Q56:** What does `rpm -V package` do?
<details>
<summary>Answer</summary>

Verifies package integrity by checking if files have been modified since installation

</details>

---

**Q57:** How do you find which package owns `/bin/bash`?
<details>
<summary>Answer</summary>

`rpm -qf /bin/bash`

</details>

---

**Q58:** What command rebuilds the RPM database?
<details>
<summary>Answer</summary>

`sudo rpm --rebuilddb`

</details>

---

## Bonus: Package Management True/False

**Q59-65:** True or False?

<details>
<summary>Questions</summary>

59. YUM automatically resolves package dependencies.
60. The rpm command can download packages from repositories.
61. `dnf` is the next generation of `yum`.
62. In RHEL 8, `yum` is a symlink to `dnf`.
63. Package groups let you install related packages together.
64. `dnf history undo` can reverse a previous installation.
65. The Software window shows all available packages.

</details>

<details>
<summary>Answers</summary>

59. **TRUE** - YUM/DNF resolves dependencies automatically
60. **FALSE** - rpm works with local files only
61. **TRUE** - DNF = Dandified YUM
62. **TRUE** - `yum` → `dnf` symlink
63. **TRUE** - Groups bundle related packages
64. **TRUE** - Can undo entire transactions
65. **FALSE** - Only shows desktop applications

</details>

---

## Topic 12: User Account Management

**Q66:** What command creates a new user account?
<details><summary>Answer</summary>

`useradd username` or `sudo useradd username`

</details>

---

**Q67:** Where are encrypted passwords stored?
<details><summary>Answer</summary>

`/etc/shadow`

</details>

---

**Q68:** What is the default skeleton directory?
<details><summary>Answer</summary>

`/etc/skel/`

</details>

---

**Q69:** What's the difference between `usermod -G` and `usermod -aG`?
<details><summary>Answer</summary>

- `-G`: Replaces ALL supplementary groups
- `-aG`: Adds to existing supplementary groups (safer!)

</details>

---

**Q70:** How do you delete a user and their home directory?
<details><summary>Answer</summary>

`sudo userdel -r username`

</details>

---

## Topic 13: Group Management

**Q71:** What is a user's primary group?
<details><summary>Answer</summary>

The default group assigned to files/directories created by that user

</details>

---

**Q72:** What command creates a new group?
<details><summary>Answer</summary>

`sudo groupadd groupname`

</details>

---

**Q73:** How do you add a user to a supplementary group?
<details><summary>Answer</summary>

`sudo usermod -aG groupname username`

</details>

---

**Q74:** What command temporarily changes your primary group?
<details><summary>Answer</summary>

`newgrp groupname`

</details>

---

## Topic 14: Advanced Permissions (ACLs)

**Q75:** What command setsACLs on a file?
<details><summary>Answer</summary>

`setfacl`

Example: `setfacl -m u:bill:rw file.txt`

</details>

---

**Q76:** How do you view ACLs on a file?
<details><summary>Answer</summary>

`getfacl filename`

</details>

---

**Q77:** What does the `+` sign in `ls -l` output indicate?
<details><summary>Answer</summary>

ACLs are set on that file

</details>

---

**Q78:** What does the set GID bit do on a directory?
<details><summary>Answer</summary>

Files created in that directory inherit the directory's group

</details>

---

**Q79:** What does the sticky bit do on a directory?
<details><summary>Answer</summary>

Only the file owner (and root) can delete files in that directory

</details>

---

**Q80:** How do you enable the set GID bit on a directory?
<details><summary>Answer</summary>

`chmod 2775 directory` or `chmod g+s directory`

</details>

---

## Bonus: User/Group True/False

**Q81-85:** True or False?

<details><summary>Questions</summary>

81. Regular user UIDs start at 1000 in RHEL/Fedora.
82. The root user has UID 0.
83. A user can only belong to one group.
84. ACL mask limits maximum permissions for ACL users.
85. The sticky bit is commonly used on /tmp.

</details>

<details><summary>Answers</summary>

81. **TRUE** - Regular UIDs start at 1000
82. **TRUE** - Root UID is always 0
83. **FALSE** - Can belong to multiple groups
84. **TRUE** - Mask = max ACL permission
85. **TRUE** - /tmp has sticky bit (drwxrwxrwt)

</details>

---

## Topic 15: Disk Storage

**Q86:** What is swap space used for?
<details><summary>Answer</summary>

Extends system memory when RAM is full by temporarily storing inactive data on disk

</details>

---

**Q87:** What's the maximum partition size for MBR?
<details><summary>Answer</summary>

**2TB** (GPT supports up to 9.4ZB)

</details>

---

**Q88:** How many primary partitions can GPT support?
<details><summary>Answer</summary>

**128** primary partitions (MBR supports only 4)

</details>

---

**Q89:** What device represents the first SCSI/SATA/USB disk?
<details><summary>Answer</summary>

`/dev/sda`

</details>

---

## Topic 16: Disk Partitioning

**Q90:** Which tool supports GPT partitions better?
<details><summary>Answer</summary>

**parted** (fdisk also supports GPT in newer versions, but parted is preferred)

</details>

---

**Q91:** What partition type ID is used for Linux swap?
<details><summary>Answer</summary>

**82**

</details>

---

**Q92:** What partition type ID is used for Linux LVM?
<details><summary>Answer</summary>

**8e**

</details>

---

**Q93:** True or False: parted changes take effect immediately.
<details><summary>Answer</summary>

**TRUE** - Unlike fdisk,  parted writes changes immediately (no undo!)

</details>

---

## Topic 17: LVM

**Q94:** What are the three components of LVM?
<details><summary>Answer</summary>

1. **Physical Volume (PV)** - Disk partitions
2. **Volume Group (VG)** - Pool of PVs
3. **Logical Volume (LV)** - Virtual partitions from VG

</details>

---

**Q95:** What command creates a physical volume?
<details><summary>Answer</summary>

`pvcreate /dev/sdb6`

</details>

---

**Q96:** How do you create a 5GB logical volume named "data" in VG "myvg"?
<details><summary>Answer</summary>

`lvcreate -n data -L 5G myvg`

</details>

---

**Q97:** True or False: You must unmount an LV to grow it.
<details><summary>Answer</summary>

**FALSE** - LVs can be grown while mounted!

</details>

---

## Topic 18: Filesystems

**Q98:** What file defines filesystems to mount at boot?
<details><summary>Answer</summary>

`/etc/fstab`

</details>

---

**Q99:** How do you view all UUIDs on your system?
<details><summary>Answer</summary>

`blkid`

</details>

---

**Q100:** What command creates an ext4 filesystem?
<details><summary>Answer</summary>

`mkfs -t ext4 /dev/sdb1`

</details>

---

**Q101:** What command mounts /dev/sdb1 on /mnt/test?
<details><summary>Answer</summary>

`mount /dev/sdb1 /mnt/test`

</details>

---

**Q102:** How do you enable a swap partition?
<details><summary>Answer</summary>

```bash
mkswap /dev/sdb2      # Format
swapon /dev/sdb2      # Enable
```

</details>

---

## Bonus: Storage True/False

**Q103-105:** True or False?

<details><summary>Questions</summary>

103. LVM allows you to span a logical volume across multiple disks.
104. You can mount an ISO image without burning it to DVD.
105. The default filesystem for RHEL 8 is ext4.

</details>

<details><summary>Answers</summary>

103. **TRUE** - LVM volume groups can include multiple physical volumes
104. **TRUE** - Use `mount -o loop image.iso /mnt`
105. **FALSE** - RHEL 8 uses xfs as default

</details>

---

**🎉 PART 3 COMPLETE! 105 Questions Total! 🎉**

*Quiz based on Linux Bible 10th Edition - Part 3: Becoming a Linux System Administrator*
