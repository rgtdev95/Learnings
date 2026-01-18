# Creating Filesystems

## mkfs Command

**Purpose:** Create a filesystem on a partition

**Syntax:**
```bash
mkfs -t <type> <device>
```

---

## Common Filesystem Types

### Create ext4 (Default for many distros)

```bash
sudo mkfs -t ext4 /dev/sdb1
```

### Create xfs (Default for RHEL/CentOS 7+)

```bash
sudo mkfs -t xfs /dev/sdb1
```

### Create ext2 (No journaling)

```bash
sudo mkfs /dev/sdb1
# OR
sudo mkfs -t ext2 /dev/sdb1
```

### Create VFAT (Windows compatible)

```bash
sudo mkfs -t vfat /dev/sdb1
```

### Create exFAT (For large files on USB)

```bash
sudo mkfs.exfat /dev/sdb1
```

---

## Before Creating Filesystem

✅ **Checklist:**
1. Partition the disk correctly
2. Get device name right (`/dev/sdb1` not `/dev/sda1`!)
3. Unmount partition if mounted
4. **Backup data** - mkfs destroys everything!

---

## mkfs Example Output

### ext4 Filesystem

```bash
$ sudo mkfs -t ext4 /dev/sdc2
mke2fs 1.44.6 (5-Mar-2019)
Creating filesystem with 524288 4k blocks and 131072 inodes
Filesystem UUID: 6379d82e-fa25-4160-8ffa-32bc78d410ee
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```

### xfs Filesystem

```bash
$ sudo mkfs -t xfs /dev/sdc1
meta-data=/dev/sdc1
         isize=256    agcount=4, agsize=256825 blks
data     =           bsize=4096   blocks=1027300
naming   =version 2  bsize=4096   ascii-ci=0
log      =internal   bsize=4096   blocks=2560
```

---

## Filesystem-Specific Commands

### Instead of `mkfs -t`:

```bash
# XFS
sudo mkfs.xfs /dev/sdb1

# ext4
sudo mkfs.ext4 /dev/sdb1

# VFAT
sudo mkfs.vfat /dev/sdb1

# exFAT
sudo mkfs.exfat /dev/sdb1

# NTFS
sudo mkfs.ntfs /dev/sdb1
```

---

## After Creating Filesystem

**1. Create mount point:**
```bash
sudo mkdir /mnt/mydata
```

**2. Mount filesystem:**
```bash
sudo mount /dev/sdb1 /mnt/mydata
```

**3. Verify:**
```bash
df -h /mnt/mydata
```

**4. Use it:**
```bash
cd /mnt/mydata
sudo touch testfile.txt
ls -l
```

---

## Making Mount Permanent

Add to `/etc/fstab`:

```
/dev/sdb1  /mnt/mydata  ext4  defaults  1  2
```

**Or use UUID:**
```bash
# Get UUID
sudo blkid /dev/sdb1

# Add to /etc/fstab
UUID=xxx-xxx-xxx  /mnt/mydata  ext4  defaults  1  2
```

---

## Managing Storage with Cockpit

**Access:** `http://hostname:9090`

### Cockpit Storage Features

**1. Storage Overview Tab:**
*   Read/write activity charts
*   Mounted filesystems
*   LVM volume groups
*   RAID devices
*   NFS shares
*   Physical storage devices

**2. View Partitions:**
*   Select a filesystem
*   See all partitions on device
*   View partition table type

**3. Reformat Device:**
*   Select "Create Partition Table"
*   Choose GPT or MBR
*   Set erase policy
*   Click Format

**4. Create Partitions:**
*   Click "Create Partition"
*   Set size
*   Choose filesystem type
*   Set encryption (optional)
*   Choose mount point
*   Click Create

**5. Advantages:**
*   Visual interface
*   Less error-prone than CLI
*   See storage layout clearly
*   Intuitive partition creation

---

## Filesystem Labels

### Add Label During Creation

```bash
# ext4
sudo mkfs -t ext4 -L MyData /dev/sdb1

# xfs
sudo mkfs.xfs -L MyData /dev/sdb1
```

### Use Label in /etc/fstab

```
LABEL=MyData  /mnt/mydata  ext4  defaults  1  2
```

---

## Checking Filesystems

### Check ext4 Filesystem

```bash
# Unmount first!
sudo umount /dev/sdb1

# Check filesystem
sudo fsck /dev/sdb1

# Force check
sudo fsck -f /dev/sdb1
```

### Check xfs Filesystem

```bash
# XFS check (can be mounted)
sudo xfs_repair /dev/sdb1
```

---

## Resizing Filesystems

### Grow ext4

```bash
# After growing partition or LV
sudo resize2fs /dev/sdb1
```

### Grow xfs

```bash
# Must be mounted!
sudo xfs_growfs /mnt/mountpoint
```

---

## Review Questions

1. What command creates a filesystem?

2. What happens to data when you run mkfs?

3. How do you create an xfs filesystem on /dev/sdb1?

4. True or False: You should unmount a partition before running mkfs.

5. Where do you configure filesystems to mount at boot?

6. What's the advantage of using UUID instead of device names?

7. Which web UI can manage storage in RHEL/Fedora?

8. How do you check an ext4 filesystem for errors?

---

## Quick Reference

```
Create Filesystems:
├── mkfs -t ext4 /dev/sdb1   → ext4
├── mkfs -t xfs /dev/sdb1    → xfs
├── mkfs -t vfat /dev/sdb1   → FAT32
└── mkfs.exfat /dev/sdb1     → exFAT

Using Filesystem:
├── mkdir /mnt/data          → Create mount point
├── mount /dev/sdb1 /mnt/data → Mount
├── df -h /mnt/data          → Check
└── umount /mnt/data         → Unmount

Make Permanent:
└── Edit /etc/fstab:
    /dev/sdb1  /mnt/data  ext4  defaults  1  2
    OR
    UUID=xxx   /mnt/data  ext4  defaults  1  2

Cockpit Storage:
└── http://hostname:9090
    ├── Storage tab
    ├── Select device
    ├── Create Partition Table
    └── Create Partition

Resize Filesystems:
├── resize2fs /dev/sdb1      → ext4
└── xfs_growfs /mount/point  → xfs
```
