# Rescue Mode

## What is Rescue Mode?

**Purpose:** Fix unbootable Linux systems

**How it works:**
1. Boot from rescue media (USB/CD/DVD)
2. Rescue system mounts your filesystems
3. You repair the broken system
4. Reboot to fixed system

**When to use:**
*   System won't boot
*   Corrupted filesystems
*   Broken boot loader
*   Missing critical files
*   Forgotten root password

---

## Entering Rescue Mode

**Requirements:**
*   Installation media (USB/CD/DVD)
*   Ability to boot from that media

**RHEL 8 Example:**

**1. Insert installation media and reboot**

**2. Select boot device:**
*   Press F12 (or F2) during BIOS
*   Choose CD/DVD or USB drive

**3. From boot menu:**
*   Highlight "Troubleshooting"
*   Press Enter
*   Select "Rescue a Red Hat Enterprise Linux system"
*   Press Enter

**4. Configure rescue environment:**
*   Select language
*   Select keyboard
*   Choose network configuration (if needed)

**5. Mount filesystems:**
*   Select "Continue" to mount under `/mnt/sysimage`
*   Or "Skip" if filesystems are damaged

**6. Get root shell:**
*   Select "OK"
*   You now have a root shell (#)

---

## Using chroot

**Purpose:** Make installed system the root filesystem

**Check mounted filesystems:**
```bash
ls /mnt/sysimage
```

**Change root:**
```bash
chroot /mnt/sysimage
```

**Now:**
*   `/` is your installed system
*   Can run commands as if booted normally
*   Can edit configuration files
*   Can reinstall packages

---

## Common Rescue Tasks

### Fix /etc/fstab

**Problem:** Bad fstab entry prevents boot

**Solution:**
```bash
chroot /mnt/sysimage
vim /etc/fstab
# Fix incorrect entries
mount -a  # Test all mounts
exit
reboot
```

**Common fstab errors:**
*   Wrong device name
*   Non-existent mount point
*   Incorrect filesystem type
*   Bad mount options

### Reinstall Missing Components

**Problem:** Critical file deleted

**Example:** Missing /bin/mount

**Solution:**
```bash
chroot /mnt/sysimage
yum reinstall util-linux
exit
reboot
```

### Check Filesystems

**Problem:** Corrupted filesystem

**Solution:**
```bash
# Don't chroot for this
fsck /dev/sda1
fsck /dev/mapper/rhel-root
```

**fsck options:**
*   `-y` - Automatically fix errors
*   `-n` - No changes (dry run)
*   `-f` - Force check

**⚠️ Warning:** Never run fsck on mounted filesystem

### Reset Root Password

**Problem:** Forgotten root password

**Solution:**
```bash
chroot /mnt/sysimage
passwd root
# Enter new password
exit
reboot
```

### Reinstall Boot Loader

**Problem:** GRUB boot loader damaged

**Solution:**
```bash
chroot /mnt/sysimage
grub2-install /dev/sda
grub2-mkconfig -o /boot/grub2/grub.cfg
exit
reboot
```

---

## Rescue Mode Without Installation Media

**Alternative:** Single-user mode

**GRUB 2 method:**
1. Interrupt GRUB (press any key)
2. Highlight kernel entry
3. Press `e` to edit
4. Find line starting with `linux`
5. Add `init=/bin/bash` to end
6. Press Ctrl+x to boot

**Result:**
*   Boots directly to shell
*   Root filesystem mounted read-only
*   No other services running

**Remount read-write:**
```bash
mount -o remount,rw /
```

**Fix problem, then reboot:**
```bash
exec /sbin/init  # Start normal boot
# Or
reboot -f        # Force reboot
```

---

## Exiting Rescue Mode

**When finished:**
```bash
exit  # Exit chroot if used
reboot
```

**Remove rescue media:**
*   Eject CD/DVD
*   Remove USB drive
*   Before system restarts

---

## Rescue Mode Limitations

**Cannot fix:**
*   Hardware failures
*   Severely corrupted disks
*   Encrypted filesystems (without key)
*   Physical damage

**May need:**
*   Professional data recovery
*   Disk replacement
*   Backup restoration

---

## Review Questions

1. What is rescue mode used for?

2. How do you enter rescue mode in RHEL?

3. What does chroot do?

4. How do you reset the root password in rescue mode?

5. What command checks filesystem integrity?

6. True or False: You can run fsck on a mounted filesystem.

7. How do you remount root filesystem read-write?

8. What kernel parameter boots directly to a shell?

---

## Quick Reference

```
Entering Rescue Mode (RHEL):
1. Boot from installation media
2. Select Troubleshooting
3. Select Rescue mode
4. Configure language/keyboard
5. Mount filesystems (Continue)
6. Get root shell

Using chroot:
├── Check: ls /mnt/sysimage
├── Enter: chroot /mnt/sysimage
├── Work: (run commands)
└── Exit: exit

Common Tasks:
├── Fix fstab: vim /etc/fstab && mount -a
├── Reset password: passwd root
├── Reinstall package: yum reinstall package
├── Check filesystem: fsck /dev/sda1
└── Reinstall GRUB: grub2-install /dev/sda

Single-User Mode:
1. Interrupt GRUB
2. Edit kernel line
3. Add: init=/bin/bash
4. Boot: Ctrl+x
5. Remount: mount -o remount,rw /

Filesystem Check:
├── fsck /dev/sda1
├── fsck -y /dev/sda1  → Auto-fix
├── fsck -n /dev/sda1  → Dry run
└── ⚠️ Never on mounted filesystem

Exit Rescue:
├── exit    → Exit chroot
├── reboot  → Restart system
└── Remove rescue media before restart

Limitations:
├── Cannot fix hardware failures
├── Cannot recover severely corrupted disks
├── Need key for encrypted filesystems
└── May need professional data recovery
```
