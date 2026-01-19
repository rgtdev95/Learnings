# Boot Troubleshooting

## Boot Process Overview

**PC Boot Sequence:**
1. Power on
2. BIOS/UEFI firmware starts
3. Boot loader found and started
4. Operating system selected
5. Kernel and initrd loaded
6. Init system starts (init or systemd)
7. Services start (runlevel or target)

---

## BIOS vs UEFI

**BIOS (Basic Input Output System):**
*   Traditional PC firmware
*   Initializes hardware
*   Finds boot loader in MBR
*   Limited to 2TB disks

**UEFI (Unified Extensible Firmware Interface):**
*   Modern replacement for BIOS
*   Supports secure boot
*   Supports disks >2TB
*   Faster boot times

**Secure Boot:**
*   Only signed OS components can boot
*   Supported in Ubuntu 12.04.2+, RHEL 7+
*   Can be disabled if needed

---

## BIOS Setup Utility

**Accessing Setup:**
*   Press F1, F2, or F12 during boot
*   Varies by manufacturer

**Common Uses:**

**View hardware overview:**
*   System model
*   BIOS version
*   Processors
*   Memory slots/types
*   32-bit vs 64-bit
*   Attached devices

**Enable/disable devices:**
*   Network interface cards
*   Audio cards
*   USB ports
*   CD/DVD drives

**Change device settings:**
*   NIC PXE boot settings
*   Virtualization support (Intel VT/AMD SVM)
*   Performance options

---

## Boot Order

**Purpose:** Define order of boot devices

**Typical order:**
1. CD/DVD drive
2. Hard disk
3. USB device
4. Network interface card (PXE)

**When to change:**

**Rescue mode:**
*   Boot from CD/USB to repair system

**Fresh install:**
*   Boot from installation media

**Testing:**
*   Boot from different device temporarily

---

## GRUB Legacy Boot Loader (RHEL 6)

**Common failures:**

**"Could not locate active partition":**
*   No bootable partition found
*   Fix: Use `fdisk` to mark partition bootable

**"Selected boot device not available":**
*   MBR deleted or overwritten
*   Try booting from rescue media
*   May need to reinstall boot loader

**Text-based GRUB prompt:**
*   MBR found, but grub.conf missing
*   Workaround: Manually enter boot commands

**Manual boot example:**
```
grub> cat (hd0,0)/grub/grub.conf
grub> root (hd0,0)
grub> kernel /vmlinuz-... root=/dev/...
grub> initrd /initrd-...
grub> boot
```

---

## Interrupting GRUB Boot

**Why interrupt:**
*   Boot to different runlevel
*   Select different kernel
*   Boot different OS
*   Change boot options

**How to interrupt:**
*   Press any key during countdown

**Boot to single-user mode:**
1. Highlight kernel entry
2. Press `e` to edit
3. Highlight kernel line
4. Press `e` to edit
5. Add ` 1` or ` single` to end
6. Press Enter
7. Press `b` to boot

**Change boot options:**
*   Add `init=/bin/bash` - Direct to shell
*   Add `nousb` - Disable USB
*   Add `selinux=0` - Disable SELinux

---

## GRUB 2 Boot Loader (RHEL 7+, Fedora, Ubuntu)

**Interrupt boot:**
1. Press any key after BIOS
2. See menu of available kernels

**Edit boot options:**
1. Highlight kernel entry
2. Press `e` to edit
3. Find line starting with `linux`
4. Add options to end of line
5. Press Ctrl+x to boot

**Common kernel parameters:**

**selinux=0:**
*   Disable SELinux temporarily

**nosmt or smt=1:**
*   Disable Simultaneous Multithreading
*   Fix for CPU vulnerabilities

**init=/bin/bash:**
*   Boot directly to shell
*   Bypass normal init

---

## Kernel Startup

**What happens:**
*   Kernel loads drivers
*   Detects hardware
*   Mounts root filesystem
*   Starts init system

**Viewing kernel messages:**
```bash
dmesg > /tmp/kernel_msg.txt
less /tmp/kernel_msg.txt
```

**With systemd:**
```bash
journalctl -k
```

**What to look for:**
*   Failed drivers
*   Hardware detection errors
*   Missing modules
*   Filesystem mount failures

---

## Review Questions

1. What are the 7 steps of the PC boot process?

2. What's the difference between BIOS and UEFI?

3. How do you access BIOS Setup?

4. What key interrupts the GRUB boot loader?

5. How do you boot to single-user mode in GRUB Legacy?

6. What does `init=/bin/bash` do?

7. What command views kernel boot messages?

8. True or False: UEFI supports disks larger than 2TB.

---

## Quick Reference

```
Boot Sequence:
1. Power on
2. BIOS/UEFI
3. Boot loader
4. Kernel
5. Init system
6. Services
7. Login

BIOS Access:
└── Press F1, F2, or F12 during boot

GRUB Legacy:
├── Interrupt: Press any key
├── Edit: e
├── Boot: b
└── Single-user: Add "1" or "single" to kernel line

GRUB 2:
├── Interrupt: Press any key
├── Edit: e
├── Boot: Ctrl+x
└── Kernel line starts with "linux"

Kernel Parameters:
├── selinux=0        → Disable SELinux
├── init=/bin/bash   → Direct to shell
├── nousb            → Disable USB
├── nosmt            → Disable SMT
└── 1 or single      → Single-user mode

View Kernel Messages:
├── dmesg
└── journalctl -k

Boot Order:
1. CD/DVD
2. Hard disk
3. USB
4. Network (PXE)
```
