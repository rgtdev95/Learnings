# Filesystems and Mounting

## Supported Filesystems

### Linux Native

*   **xfs** - Default for RHEL 7+, high performance, large files
*   **ext4** - Most common, successor to ext3, journaling
*   **ext3** - Older, journaling, stable
*   **ext2** - Original, no journaling
*   **btrfs** - Copy-on-write, advanced features

### Windows Compatible

*   **vfat** - FAT32, compatible with Windows/Mac/Linux
*   **exfat** - Extended FAT, optimized for flash drives
*   **ntfs** - Windows NT filesystem (read-only or with ntfs-3g)

### Network

*   **nfs** - Network File System (Unix/Linux)
*   **cifs** - Common Internet Filesystem (Windows/Samba)

### Special

*   **swap** - Swap space (not a filesystem)
*   **iso9660** - CD/DVD
*   **tmpfs** - RAM-based temporary filesystem

**View loaded filesystems:**
```bash
cat /proc/filesystems
```

---

## The /etc/fstab File

**Purpose:** Define filesystems to mount at boot

**Format:** 6 fields per line

```
Device  MountPoint  FSType  Options  Dump  fsck
```

### Example /etc/fstab

```
# Device/UUID                  Mount    Type  Options      Dump fsck
/dev/mapper/vg_abc-lv_root     /        ext4  defaults     1    1
UUID=78bdae46-9389...          /boot    ext4  defaults     1    2
/dev/mapper/vg_abc-lv_home     /home    ext4  defaults     1    2
/dev/mapper/vg_abc-lv_swap     swap     swap  defaults     0    0
/dev/sdb1                      /win     vfat  ro           1    2
192.168.0.27:/nfsstuff         /remote  nfs   _netdev      0    0
//192.168.0.28/share           /share   cifs  guest,_netdev 0   0
```

### Field Breakdown

**Field 1: Device**
*   Device name: `/dev/sda1`
*   UUID: `UUID=78bdae46...`
*   Label: `LABEL=MyDisk`
*   Network: `server:/path` or `//server/share`

**Field 2: Mount Point**
*   Directory where filesystem mounts
*   Example: `/`, `/home`, `/mnt/usb`
*   Use `swap` for swap partitions

**Field 3: Filesystem Type**
*   `ext4`, `xfs`, `vfat`, `ntfs`, `nfs`, `cifs`, `swap`

**Field 4: Mount Options**
*   `defaults` - Standard options
*   `ro` - Read-only
*   `rw` - Read-write
*   `user` - Allow non-root to mount
*   `_netdev` - Wait for network before mounting
*   `noauto` - Don't mount at boot

**Field 5: Dump**
*   `0` - Don't backup with dump
*   `1` - Backup with dump
*   (Rarely used nowadays)

**Field 6: fsck Order**
*   `0` - Don't check
*   `1` - Check first (root filesystem)
*   `2` - Check after `1` completes

---

## UUID vs Device Names

**Problem:** Device names can change (sdb → sdc)

**Solution:** Use UUID (Universally Unique Identifier)

### View UUIDs

```bash
sudo blkid
```

**Output:**
```
/dev/sda1: UUID="78bdae46-9389-438d-bfee-06dd934fae28" TYPE="ext4"
/dev/sdb1: UUID="75E0-96AA" TYPE="vfat"
```

### Using UUID in /etc/fstab

```
UUID=78bdae46-9389-438d-bfee-06dd934fae28 /boot ext4 defaults 1 2
```

**Advantage:** Partition always found, even if device name changes

---

## Mounting Filesystems

### View Mounted Filesystems

```bash
mount
```

**Or check specific:**
```bash
mount | grep sdb
df -h
```

### Mount Manually

```bash
# Create mount point
sudo mkdir /mnt/temp

# Mount partition
sudo mount /dev/sdb1 /mnt/temp

# Verify
df -h /mnt/temp
```

### Mount with Options

```bash
# Mount read-only
sudo mount -o ro /dev/sdb1 /mnt/temp

# Mount with specific filesystem type
sudo mount -t ext4 /dev/sdb1 /mnt/temp
```

### Remount

```bash
# Remount as read-write
sudo mount -o remount,rw /dev/sdb1
```

### Mount All from /etc/fstab

```bash
sudo mount -a
```

---

## Unmounting Filesystems

### Basic Unmount

```bash
sudo umount /mnt/temp
# OR
sudo umount /dev/sdb1
```

### If "Device is Busy"

**Find what's using it:**
```bash
sudo lsof /mnt/temp
sudo fuser -v /mnt/temp
```

**Lazy unmount:**
```bash
sudo umount -l /mnt/temp
```

**Force unmount (NFS):**
```bash
sudo umount -f /mnt/nfs
```

---

## Swap Areas

### Create Swap Partition

```bash
# Format as swap
sudo mkswap /dev/sdb2

# Enable swap
sudo swapon /dev/sdb2

# Check swap
free -m
```

### Create Swap File

```bash
# Create 1GB file
sudo dd if=/dev/zero of=/var/tmp/myswap bs=1M count=1024

# Format as swap
sudo mkswap /var/tmp/myswap

# Enable swap
sudo swapon /var/tmp/myswap

# Verify
free -m
```

### Make Swap Permanent

Add to `/etc/fstab`:
```
/dev/sdb2      swap  swap  defaults  0  0
/var/tmp/myswap swap  swap  defaults  0  0
```

### Enable All Swap

```bash
sudo swapon -a
```

### Disable Swap

```bash
sudo swapoff /dev/sdb2
sudo swapoff /var/tmp/myswap
```

---

## Mounting ISO Images

**Mount disk image without burning:**

```bash
sudo mkdir /mnt/iso
sudo mount -o loop myimage.iso /mnt/iso
cd /mnt/iso
ls
```

**Unmount when done:**
```bash
sudo umount /mnt/iso
```

---

## Common Mount Options

| Option | Meaning |
|--------|---------|
| `defaults` | rw, suid, dev, exec, auto, nouser, async |
| `ro` | Read-only |
| `rw` | Read-write |
| `user` | Allow regular users to mount |
| `nouser` | Only root can mount |
| `auto` | Mount at boot with `mount -a` |
| `noauto` | Don't mount at boot |
| `exec` | Allow executables |
| `noexec` | Disallow executables |
| `_netdev` | Network device (wait for network) |

---

## Review Questions

1. What file defines filesystems to mount at boot?

2. How do you view all UUIDs on your system?

3. What mount option makes a filesystem read-only?

4. How do you enable a swap partition temporarily?

5. True or False: You can mount an ISO image without burning it.

6. What command unmounts a filesystem?

7. Which field in /etc/fstab sets mount options?

8. What does `_netdev` option do?

---

## Quick Reference

```
View Filesystems:
├── cat /proc/filesystems  → Loaded types
├── mount                  → Currently mounted
├── df -h                  → Disk usage
└── blkid                  → UUIDs

Mount:
├── mount /dev/sdb1 /mnt   → Basic mount
├── mount -t xfs ...       → Specify type
├── mount -o ro ...        → Mount read-only
├── mount -o loop iso /mnt → Mount ISO
└── mount -a               → Mount all from fstab

Unmount:
├── umount /mnt            → Basic unmount
├── umount -l /mnt         → Lazy unmount
├── lsof /mnt              → See what's using it
└── fuser -v /mnt          → See processes

Swap:
├── mkswap /dev/sdb2       → Format as swap
├── swapon /dev/sdb2       → Enable swap
├── swapoff /dev/sdb2      → Disable swap
├── swapon -a              → Enable all swap
└── free -m                → View swap usage

/etc/fstab Format:
Device  MountPoint  Type  Options  Dump  fsck
```
