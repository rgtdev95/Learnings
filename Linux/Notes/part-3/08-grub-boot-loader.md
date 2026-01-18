# GRUB Boot Loader

## Key Concepts

| Term | Definition |
|------|------------|
| **Boot Loader** | Software that loads the operating system |
| **GRUB** | GRand Unified Bootloader |
| **MBR** | Master Boot Record (where boot loader lives) |
| **Kernel** | Core of the operating system |
| **initrd** | Initial RAM disk (drivers/tools for boot) |

---

## What is a Boot Loader?

A **boot loader** lets you choose when and how to boot installed operating systems.

**Functions:**
*   Present menu of bootable operating systems
*   Load selected OS kernel
*   Pass boot options to kernel
*   Support multi-boot configurations

---

## GRUB Versions

### GRUB Legacy (Version 1)

**Used by:** Older RHEL, Fedora, Ubuntu releases

**Configuration:** `/boot/grub/grub.conf`

**Characteristics:**
*   Simpler configuration
*   Manual editing of `grub.conf`
*   Less flexible

### GRUB 2 (Current Version)

**Used by:** RHEL 8+, Fedora 16+, Ubuntu 9.10+

**Configuration:** 
*   `/boot/grub2/grub.cfg` (BIOS systems)
*   `/boot/efi/EFI/redhat/grub.cfg` (UEFI systems)

**Characteristics:**
*   Auto-generated config file
*   Scripting support (functions, loops, variables)
*   More reliable device naming (UUIDs/labels)
*   Modular design

---

## GRUB 2 Configuration

### File Locations

| File/Directory | Purpose |
|----------------|---------|
| `/boot/grub2/grub.cfg` | Main config (auto-generated, don't edit) |
| `/etc/default/grub` | GRUB defaults (edit this) |
| `/etc/grub.d/` | Scripts that generate `grub.cfg` |
| `/boot/loader/entries/` | Boot menu entries (RHEL/Fedora) |

### Important: Don't Edit grub.cfg Directly!

Instead, modify:
1. `/etc/default/grub` (for global settings)
2. `/etc/grub.d/` scripts (for menu generation)
3. Run `grub2-mkconfig` to regenerate `grub.cfg`

---

## Boot Menu Entries

### Location: `/boot/loader/entries/`

Each `.conf` file creates a menu entry.

**Example:** `/boot/loader/entries/xxx-4.18.0-80.el8.x86_64.conf`

```
title Red Hat Enterprise Linux (4.18.0-80.el8.x86_64) 8.0 (Ootpa)
version 4.18.0-80.el8.x86_64
linux /vmlinuz-4.18.0-80.el8.x86_64
initrd /initramfs-4.18.0-80.el8.x86_64.img
options $kernelopts
id rhel-20190313123447-4.18.0-80.el8.x86_64
grub_users $grub_users
grub_arg --unrestricted
grub_class kernel
```

**Key Lines:**
*   `title`: Boot menu text
*   `linux`: Kernel path
*   `initrd`: Initial RAM disk path
*   `options`: Kernel boot options

---

## Common Kernel Boot Options

### Changing Run Levels

Add to end of `linux` line:

| Option | Effect |
|--------|--------|
| `1` | Single-user mode (rescue) |
| `3` | Multi-user + networking (no GUI) |
| `5` | Full GUI mode |

**Use Case:** Boot to level 3 if GUI is broken

### Debugging Options

| Option | Effect |
|--------|--------|
| `rhgb quiet` | (Default) Show splash screen, hide messages |
| *(remove above)* | Show boot messages scrolling |
| `systemd.unit=rescue.target` | Boot to rescue mode |
| `systemd.unit=multi-user.target` | Boot to multi-user (no GUI) |

### Recovery Options

**Forgot Root Password?**
1. Boot system
2. Press `e` at GRUB menu
3. Find line starting with `linux`
4. Add `rd.break` to end of line
5. Press Ctrl+X to boot
6. System drops to emergency shell before mounting root
7. Reset password from rescue environment

---

## Device Naming in GRUB 2

GRUB 2 uses more reliable device identification:

### Old Method (Device Names)
```
/dev/sda1    # Can change if disks added/removed
```

### New Method (UUIDs)
```
UUID=a8b4c6d8-1234-5678-90ab-cdef12345678
```

### Alternative (Labels)
```
LABEL=myroot
```

**Advantage:** Disk won't fail to boot if device names change

---

## Editing GRUB Defaults

### File: `/etc/default/grub`

**Example:**
```
GRUB_TIMEOUT=5
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="rhgb quiet"
```

**Common Settings:**
*   `GRUB_TIMEOUT`: Seconds before auto-boot (0=instant, -1=wait forever)
*   `GRUB_DEFAULT`: Default menu entry (0, 1, 2... or "saved")
*   `GRUB_CMDLINE_LINUX`: Default kernel options

**After editing:**
```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

## GRUB 2 Commands

### Regenerate Configuration
```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### Set Default Boot Entry
```bash
sudo grub2-set-default 0    # First entry
sudo grub2-set-default 1    # Second entry
```

### View Default Entry
```bash
sudo grub2-editenv list
```

### Install GRUB to MBR
```bash
sudo grub2-install /dev/sda
```

---

## Multi-Boot with GRUB

GRUB 2 can boot:
*   **Linux:** Any distribution
*   **Windows:** Via chainloading
*   **BSD:** FreeBSD, OpenBSD, etc.
*   **Other:** Any bootable OS

**Example Windows Entry:**
```
menuentry "Windows 10" {
    insmod part_msdos
    insmod ntfs
    set root=(hd0,1)
    chainloader +1
}
```

---

## Other Boot Loaders

### SYSLINUX Family

**Used for:**
*   Bootable CDs/DVDs (`isolinux`)
*   USB drives (`syslinux`)
*   PXE network boot (`pxelinux`)

**Not typically used for installed systems**

---

## GRUB Interactive Mode

At boot, press:
*   **e**: Edit selected boot entry (temporary)
*   **c**: Drop to GRUB command line
*   **Esc**: Cancel and return to menu

**GRUB Command Line:**
```
grub> ls                    # List devices
grub> ls (hd0,1)/          # List files on partition
grub> cat (hd0,1)/etc/fstab  # View file
```

---

## Review Questions

1. What does GRUB stand for?

2. Which GRUB config file should you NOT edit directly?

3. Where are boot menu entries stored in RHEL 8?

4. What kernel option boots to rescue mode?

5. True or False: GRUB 2 uses UUIDs for more reliable device identification.

6. What command regenerates `grub.cfg`?

7. How do you boot to text mode (runlevel 3)?

8. Which boot loader is commonly used for bootable CDs?

---

## Quick Reference

```
GRUB 2 Files:
├── /boot/grub2/grub.cfg           → Don't edit!
├── /etc/default/grub              → Edit this
├── /etc/grub.d/                   → Generation scripts
└── /boot/loader/entries/*.conf    → Menu entries

Commands:
├── grub2-mkconfig      → Regenerate config
├── grub2-set-default   → Set default entry
├── grub2-install       → Install to MBR
└── grub2-editenv list  → View current default

Kernel Options:
├── 1 or 3 or 5        → Runlevel/target
├── rd.break           → Emergency shell
└── (remove rhgb quiet) → Show boot messages
```

