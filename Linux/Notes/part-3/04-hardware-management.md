# Hardware Management

## Checking Hardware Information

Linux provides several commands to view and manage hardware.

### Viewing Boot Messages (dmesg)

See kernel messages about hardware detection during boot.

```bash
$ dmesg | less
```

**Example Output:**
```
[    0.000000] Linux version 5.0.9-301.fc30.x86_64
[   79.177466] sd 9:0:0:0: Attached scsi generic sg2 type 0
[   79.177854] sd 9:0:0:0: [sdb] 8343552 512-byte logical blocks
```

**Continuous monitoring:**
```bash
$ dmesg -w          # Watch for new hardware events
$ tail -f /var/log/messages   # Alternative log watching
```

---

## Hardware Detection Commands

### lspci - List PCI Devices
Shows devices connected to PCI buses.

```bash
$ lspci
00:00.0 Host bridge: Intel Corporation 5000X Chipset...
00:1b.0 Audio device: Intel Corporation High Definition Audio
07:00.0 VGA compatible controller: nVidia Corporation NV44
0c:02.0 Ethernet controller: Intel Corporation 82541PI
```

**More details:**
```bash
$ lspci -v          # Verbose
$ lspci -vv         # Very verbose (driver info)
```

### lsusb - List USB Devices
Shows USB hubs and connected USB devices.

```bash
$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 413c:2105 Dell Computer Corp. Keyboard
Bus 002 Device 004: ID 413c:3012 Dell Computer Corp. Mouse
Bus 001 Device 005: ID 090c:1000 Silicon Motion 64MB USB Drive
```

### lscpu - CPU Information
```bash
$ lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
CPU(s):              4
Thread(s) per core:  1
Core(s) per socket:  4
```

---

## Managing Removable Media

Modern Linux desktops automatically handle removable media (USB drives, CDs, DVDs).

### Automatic Handling (GNOME)
1. **Insert/Connect** → Device detected by Udev
2. **Mount Point Created** → `/run/media/username/device_label`
3. **Notification** → Popup asks what to do
4. **Auto-mount** → Filesystem mounted (usually)

### Configuring Removable Media Behavior

**GNOME 3 Settings:**
*   Open: Activities → "Removable Media"
*   Settings for: Audio CD, DVD Video, Photos, Software

**Common Actions:**
*   **Audio CD:** Rhythmbox, Audio CD Extractor, Brasero
*   **DVD Video:** Totem, Brasero
*   **Photos:** Shotwell
*   **USB Drives:** Auto-mount to `/run/media/username/`

**Safely Remove:**
Right-click device in file manager → "Safely Remove Drive"

---

## Kernel Modules

Kernel modules are drivers that can be loaded/unloaded dynamically.

### Listing Loaded Modules (lsmod)
```bash
$ lsmod
Module              Size  Used by
vfat               17411  1
e1000             137260  0
usb_storage        65065  2 uas
```

### Getting Module Info (modinfo)
```bash
$ modinfo e1000
description:    Intel(R) PRO/1000 Network Driver
author:         Intel Corporation
filename:       /lib/modules/.../e1000.ko
```

### Loading Modules (modprobe)
```bash
# modprobe parport          # Load module
# modprobe parport_pc io=0x3bc irq=auto   # With options
```

**Module Location:** `/lib/modules/$(uname -r)/`

### Unloading Modules (rmmod)
```bash
# rmmod parport_pc          # Remove module
# modprobe -r parport_pc    # Remove + dependencies
```

**Cannot remove:**
*   Modules currently in use
*   Built-in kernel modules

---

## Review Questions

1. Which command shows kernel boot messages?

2. What command lists all PCI devices?

3. Which command displays connected USB devices?

4. Where are removable USB drives typically mounted?

5. Which command lists currently loaded kernel modules?

6. What does `modprobe` do?

7. True or False: You can unload any kernel module with `rmmod`.

8. How do you get detailed information about a specific module?

---

## Quick Reference

```
Hardware Info:
├── dmesg            → Kernel messages
├── lspci            → PCI devices
├── lsusb            → USB devices
└── lscpu            → CPU information

Modules:
├── lsmod            → List loaded modules
├── modinfo          → Module information
├── modprobe         → Load module
└── rmmod            → Unload module

Removable Media:
└── /run/media/user/ → Auto-mount location
```
