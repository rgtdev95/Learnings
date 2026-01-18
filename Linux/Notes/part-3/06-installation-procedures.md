# Installation Procedures and Boot Options

## Fedora Live Media Installation

### Pre-Installation Steps

1. **Get Fedora ISO** (from https://getfedora.org)
2. **Burn to DVD/USB** (see Appendix A)
3. **Backup data** (installation may erase disk)
4. **Unplug USB drives** (prevent accidental overwrite)

### Installation Steps

**1. Boot the Live Image**
*   Insert DVD/USB
*   Press F12 (or BIOS boot key) to select boot device
*   Select "Start Fedora-Workstation-Live"

**2. Start Installation**
*   Click "Install to Hard Drive" from desktop

**3. Select Language**
*   Choose language (e.g., U.S. English)
*   Click Next

**4. Time & Date**
*   Select time zone (map or drop-down)
*   Enable "Network Time" or set manually
*   Click Done

**5. Installation Destination**
*   Select target hard drive
*   Choose "Automatic" partitioning
*   OR: Select "I would like to make additional space available" to reclaim space
*   Click Done

**6. Keyboard Layout**
*   Use default or select different layout

**7. Begin Installation**
*   Click "Begin Installation"

**8. Finish Configuration**
*   Click Quit when complete

**9. Reboot**
*   Click Restart
*   Remove installation media

**10. First Boot**
*   Create user account
*   Set password
*   Account gets sudo privileges automatically

**11. Update Software**
```bash
sudo dnf update
```

---

## RHEL Installation DVD Procedure

### Pre-Installation

1. **Get RHEL ISO** (requires Red Hat account or use CentOS)
2. **Burn to DVD/USB**
3. **Boot from media**

### Installation Steps

**1. Boot Screen**
*   Select "Install" or "Test this media & install"
*   Press Tab to add boot options

**2. Language Selection**
*   Choose language and click Continue

**3. Installation Summary Screen**
*   Configure: Localization, Software, System

**4. Keyboard**
*   Choose keyboard layout

**5. Language Support**
*   Add additional languages (optional)

**6. Time & Date**
*   Set time zone
*   Enable Network Time or set manually

**7. Installation Source**
*   Default: DVD
*   OR: Network location (http/https/ftp)
*   Can add additional repositories

**8. Software Selection**
*   "Server with GUI" (default)
*   "Server" (no GUI)
*   "Minimal Install" (bare bones)
*   "Workstation" (desktop focused)

**9. Installation Destination**
*   Select local hard drive
*   Choose automatic partitioning
*   OR: Configure custom partitioning

**10. Kdump**
*   Enable for kernel crash dumps (160MB+ RAM reserved)

**11. Network & Hostname**
*   Enable network interfaces (ON)
*   Configure manually or use DHCP
*   Set hostname

**12. Security Policy**
*   Optional security compliance standard

**13. System Purpose**
*   Optional role, SLA, usage selection

**14. Begin Installation**
*   Click "Begin Installation"

**15. Root Password**
*   Set and confirm root password
*   Click Done (twice if weak password)

**16. User Creation**
*   Create non-root user
*   Set password
*   Check "Make this user administrator" for sudo access

**17. Complete Installation**
*   Click Reboot when finished

**18. First Boot (License & Registration)**
*   Accept license
*   Register with Red Hat Subscription Manager
*   Attach subscription

**19. Update System**
```bash
sudo dnf upgrade
```

---

## Installation Boot Options

Boot options modify the installer's behavior. Press **Tab** at boot screen to edit kernel line.

### Disabling Features

| Option | Effect |
|--------|--------|
| `nofirewire` | Disable FireWire support |
| `nousb` | Disable USB support |
| `nonet` | Don't probe network devices |
| `nodma` | Disable DMA for hard disks |
| `noipv6` | Disable IPv6 networking |
| `acpi=off` | Disable ACPI |
| `numa=off` | Disable NUMA for AMD64 |

### Video Options

| Option | Effect |
|--------|--------|
| `xdriver=vesa` | Use standard VESA driver |
| `resolution=1024x768` | Force specific resolution |
| `nofb` | Don't use VGA 16 framebuffer |
| `graphical` | Force graphical installation |
| `text` | Force text-mode installation |

### VNC Installation

| Option | Effect |
|--------|--------|
| `vnc` | Run as VNC server |
| `vncconnect=host:port` | Connect to VNC client |
| `vncpassword=password` | Require password (8+ chars) |

**Example:** Install and view from another computer
```
vnc vncpassword=mypass123
```
Then connect from remote: `vncviewer 192.168.0.99:1`

### Repository Options

**Syntax:**
```bash
repo=hd:/dev/sda1:/myrepo           # Hard disk
repo=http://example.com/myrepo      # Web server
repo=ftp://ftp.example.com/myrepo   # FTP server
repo=cdrom                          # CD/DVD
repo=nfs::nfs.example.com:/myrepo   # NFS share
```

### Kickstart Options

**Syntax:**
```bash
ks=cdrom:/root/ks.cfg              # CD/DVD
ks=hd:sda2:/test/ks.cfg            # Hard disk partition
ks=http://example.com/ks.cfg       # Web server
ks=ftp://ftp.example.com/ks.cfg    # FTP server
ks=nfs:nfs.example.com:/ks/ks.cfg  # NFS share
```

### Miscellaneous Options

| Option | Effect |
|--------|--------|
| `rescue` | Boot to rescue mode (not install) |
| `mediacheck` | Verify installation media integrity |
| `askmethod` | Prompt for repository location |

**Example with multiple options:**
```
vmlinuz initrd=initrd.img text ks=http://server/ks.cfg
```

---

## Multi-Boot (Dual Boot) Considerations

### Before Multi-Boot Setup

1. **Back up all data**
2. **Defragment Windows partition**
3. **Remove Windows swap file** (Control Panel → System → Performance → Virtual Memory)
4. **Check for hidden files** (`attrib -s -h`)

### Creating Free Space

**Option 1: Add Hard Disk**
*   Safest approach
*   Install Linux on separate physical disk

**Option 2: Resize Windows Partition**
*   Use tools: GParted, Acronis Disk Director
*   Free up space at end of partition
*   Risk of data loss

### Boot Loader Setup

*   Install Windows **first**, Linux **second**
*   Linux boot loader (GRUB) can boot both
*   Windows installer may damage Linux partitions

---

## Review Questions

1. What is the first thing you should do before installing Linux?

2. How do you boot into text-mode installation?

3. Which boot option tests the installation media?

4. What does the `rescue` boot option do?

5. True or False: You can resize a Windows partition with GParted.

6. What sudo command updates all packages on Fedora?

7. Which installer option allows completely automated installation?

8. For dual-boot systems, which OS should be installed first?

---

## Quick Reference

```
Boot Options:
├── text           → Text-mode install
├── rescue         → Rescue mode
├── vnc            → VNC installation
├── ks=URL         → Kickstart file
└── repo=URL       → Package repository

Post-Install:
└── sudo dnf update/upgrade  → Get updates
```

