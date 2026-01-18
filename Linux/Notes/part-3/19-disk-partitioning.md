# Disk Partitioning

## WARNING ⚠️

**Partitioning can destroy data!**
*   Practice on USB drives, not system disks
*   Double-check device names
*   Back up important data first
*   Bad /etc/fstab entry can prevent boot

---

## Partitioning Tools

| Tool | Supports GPT | Supports MBR | Best For |
|------|--------------|--------------|----------|
| **parted** | ✅ Yes | ✅ Yes | GPT, interactive & scripting |
| **fdisk** | ✅ Yes (newer versions) | ✅ Yes | MBR, interactive |
| **gdisk** | ✅ Yes | ❌ No | GPT only |

---

## Creating Single-Partition Disk (parted)

### Example: Format 128GB USB drive with one partition

**1. Insert USB and find device name:**
```bash
sudo journalctl -f
# Insert USB, watch for device name (e.g., /dev/sdb)
# Press Ctrl+C to exit
```

**2. Unmount if auto-mounted:**
```bash
mount | grep sdb
sudo umount /dev/sdb1
```

**3. Start parted:**
```bash
sudo parted /dev/sdb
```

**4. View current partitions:**
```
(parted) p
```

**5. Delete existing partitions:**
```
(parted) rm 1
```

**6. Create GPT partition table:**
```
(parted) mklabel gpt
Warning: This will destroy all data!
Yes/No? Yes
```

**7. Create new partition:**
```
(parted) mkpart
Partition name? []? alldisk
File system type? [ext2]? xfs
Start? 1MB
End? 123GB
```

**Note:** Start at 1MB (not 0) for proper alignment.

**8. Verify:**
```
(parted) p
(parted) quit
```

**9. Create filesystem:**
```bash
sudo mkfs -t xfs /dev/sdb1
```

**10. Mount:**
```bash
sudo mkdir /mnt/test
sudo mount /dev/sdb1 /mnt/test
df -h /mnt/test
```

---

## Creating Multiple-Partition Disk (fdisk)

### Example: Create MBR disk with 5 partitions

**Partition Plan:**
*   sdb1: 5GB Linux (ext4)
*   sdb2: 5GB Swap
*   sdb3: 3GB Linux (ext2)
*   sdb4: Extended (rest of disk)
*   sdb5: 3GB FAT32 (inside extended)
*   sdb6: 4GB LVM (inside extended)

### Procedure

**1. Start fdisk:**
```bash
sudo fdisk /dev/sdb
```

**2. Create first partition (5GB Linux):**
```
Command (m for help): n
Partition type: p (primary)
Partition number: 1
First sector: <Enter>
Last sector: +5G
```

**3. Create second partition (5GB):**
```
Command (m for help): n
Partition type: p
Partition number: 2
First sector: <Enter>
Last sector: +5G
```

**4. Change partition 2 to swap:**
```
Command (m for help): t
Partition number: 2
Hex code: 82
```

**5. Create third partition (3GB):**
```
Command (m for help): n
Partition type: p
Partition number: 3
First sector: <Enter>
Last sector: +3G
```

**6. Create extended partition (rest of disk):**
```
Command (m for help): n
Partition type: e
Partition number: 4
First sector: <Enter>
Last sector: <Enter>  (uses rest of disk)
```

**7. Create logical partition (3GB FAT32):**
```
Command (m for help): n
First sector: <Enter>
Last sector: +3G
```

Automatically numbered as partition 5.

**8. Change partition 5 to FAT32:**
```
Command (m for help): t
Partition number: 5
Hex code: c
```

**9. Create logical partition (4GB LVM):**
```
Command (m for help): n
First sector: <Enter>
Last sector: +4G
```

**10. Change partition 6 to LVM:**
```
Command (m for help): t
Partition number: 6
Hex code: 8e
```

**11. Review partitions:**
```
Command (m for help): p
```

**12. Write changes:**
```
Command (m for help): w
```

**13. Update kernel:**
```bash
sudo partprobe /dev/sdb
grep sdb /proc/partitions
```

---

## Formatting Partitions

After partitioning, format each partition:

```bash
# Linux ext4
sudo mkfs -t ext4 /dev/sdb1

# Swap
sudo mkswap /dev/sdb2

# Linux ext2
sudo mkfs /dev/sdb3

# VFAT
sudo mkfs -t vfat /dev/sdb5

# LVM physical volume
sudo pvcreate /dev/sdb6
```

---

## Common parted Commands

| Command | Purpose |
|---------|---------|
| `p` or `print` | Show partition table |
| `mklabel gpt` | Create GPT partition table |
| `mklabel msdos` | Create MBR partition table |
| `mkpart` | Create new partition |
| `rm N` | Delete partition N |
| `resizepart N END` | Resize partition N |
| `quit` | Exit (changes saved immediately!) |

---

## Common fdisk Commands

| Command | Purpose |
|---------|---------|
| `p` | Print partition table |
| `n` | New partition |
| `d` | Delete partition |
| `t` | Change partition type |
| `l` | List partition type codes |
| `w` | Write changes and quit |
| `q` | Quit without saving |
| `m` | Help menu |

---

## Key Differences: parted vs fdisk

| Feature | parted | fdisk |
|---------|--------|-------|
| **Changes** | Immediate | On write command |
| **GPT support** | Excellent | Good (newer versions) |
| **Scripting** | Easier | Harder |
| **Undo** | No (immediate) | Yes (before 'w') |

---

## Partition Type Codes (MBR)

Common hex codes for `fdisk` command `t`:

*   `83` - Linux
*   `82` - Linux swap
*   `8e` - Linux LVM
*   `c`  - W95 FAT32 (LBA)
*   `7`  - HPFS/NTFS/exFAT
*   `5`  - Extended
*   `b`  - W95 FAT32

**List all codes:** Type `l` in fdisk

---

## Best Practices

**Sizing partitions:**
*   Use `+5G` for 5 gigabytes
*   Use `+1024M` for 1024 megabytes
*   Don't forget the `+` sign!
*   Don't forget `M` or `G` suffix!

**Alignment:**
*   Start partitions at 1MB or later
*   Modern tools handle alignment automatically

**Safety:**
*   Always verify with `p` before writing
*   Double-check device name
*   Unmount partitions before modifying

---

## Review Questions

1. Which tool supports GPT partitions better: parted or fdisk?

2. What does partition type 82 represent?

3. In MBR, how many primary partitions can you have?

4. True or False: parted changes take effect immediately.

5. What command creates a swap partition from /dev/sdb2?

6. How do you specify a 5GB partition size in fdisk?

7. What is partition type 8e used for?

8. Which partition type contains logical partitions?

---

## Quick Reference

```
Single Partition (parted):
└── parted /dev/sdb
    ├── mklabel gpt    → Create partition table
    ├── mkpart         → Create partition
    ├── p              → View
    └── quit           → Exit

Multiple Partitions (fdisk):
└── fdisk /dev/sdb
    ├── n              → New partition
    ├── t              → Change type
    ├── p              → Print table
    └── w              → Write and exit

Format Partitions:
├── mkfs -t ext4 /dev/sdb1   → Linux filesystem
├── mkswap /dev/sdb2         → Swap
├── mkfs -t vfat /dev/sdb3   → FAT32
└── pvcreate /dev/sdb4       → LVM

Partition Types (MBR):
├── 83 → Linux
├── 82 → Swap
├── 8e → LVM
├── c  → FAT32
└── 5  → Extended
```
