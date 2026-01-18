# Disk Storage Basics

## Understanding Disk Storage

**Permanent Storage:** Hard disks (HDD/SSD) store data when computer is off

**Temporary Storage:** RAM and swap store data while computer runs

---

## Storage Hierarchy

```
Disk → Partitions → Filesystems → Mount Points → Directories/Files
```

### Example

```
/dev/sda (160GB disk)
  ├── /dev/sda1 (500MB) → /boot (ext4)
  └── /dev/sda2 (159.5GB) → LVM Physical Volume
       └── vg_abc (Volume Group)
            ├── lv_root → / (ext4)
            ├── lv_home → /home (ext4)
            └── lv_swap → swap
```

---

## RAM vs Disk vs Swap

| Storage | Speed | Cost | Persistent | Purpose |
|---------|-------|------|------------|---------|
| **RAM** | Fastest | Expensive | No (erased on reboot) | Active programs/data |
| **SSD** | Fast | Moderate | Yes | Permanent storage |
| **HDD** | Slow | Cheap | Yes | Permanent storage |
| **Swap** | Slowest | N/A | No | Extended memory when RAM full |

### Office Metaphor

*   **Disk** = File cabinet (permanent storage)
*   **RAM** = Desktop (working space)
*   **Swap** = Temporary file box (overflow from desk)

---

## Swap Space

**Purpose:** Extend system memory when RAM is full

**Types:**
*   Swap partition (dedicated disk partition)
*   Swap file (file on existing filesystem)

**How it works:**
1. RAM fills up with running processes
2. Kernel swaps out inactive data to swap space
3. When needed again, swaps data back to RAM

**Performance Impact:** Swapping is slower than RAM - avoid if possible!

---

## Partition Tables

Two main types of partition tables store partition information.

### MBR (Master Boot Record)

**Used by:** Older BIOS-based systems

**Limitations:**
*   Maximum 4 primary partitions
*   Maximum 2TB partition size
*   One partition can be "extended" to hold logical partitions

**Partition Types:**
*   Primary (up to 4)
*   Extended (1, contains logical partitions)
*   Logical (5+, inside extended partition)

### GPT (GUID Partition Table)

**Used by:** Modern UEFI-based systems

**Advantages:**
*   Up to 128 primary partitions
*   Maximum 9.4ZB partition size
*   More reliable (backup partition table)
*   No need for extended/logical partitions

---

## Device Naming

### SCSI/SATA/USB Drives

**Pattern:** `/dev/sd?`

**Examples:**
*   `/dev/sda` - First disk
*   `/dev/sdb` - Second disk
*   `/dev/sdc` - Third disk

**Partitions:**
*   `/dev/sda1` - First partition on first disk
*   `/dev/sda2` - Second partition on first disk
*   `/dev/sdb1` - First partition on second disk

**Limits:** Up to 15 partitions per disk (sda1 through sda15)

### NVMe SSD Drives

**Pattern:** `/dev/nvme?n?p?`

**Examples:**
*   `/dev/nvme0n1` - First namespace on first NVMe
*   `/dev/nvme0n1p1` - First partition
*   `/dev/nvme0n1p2` - Second partition

---

## Viewing Partitions

### Using parted

```bash
# View all disks
sudo parted -l

# View specific disk
sudo parted -l /dev/sda
```

**Output shows:**
*   Disk size
*   Partition table type (dos/gpt)
*   Partition list with start/end/size/type

### Using fdisk

```bash
# View specific disk
sudo fdisk -l /dev/sda
```

**Shows:**
*   Disk identifier
*   Partition device names
*   Boot flag (*)
*   Start/end sectors
*   Size
*   Partition type/ID

### Checking Kernel Knowledge

```bash
# See what kernel knows about partitions
cat /proc/partitions

# Force kernel to re-read partition table
sudo partprobe /dev/sdb
```

---

## Common Partition Types (IDs)

| ID | Type | Purpose |
|----|------|---------|
| **83** | Linux | Regular Linux filesystem |
| **82** | Linux swap | Swap space |
| **8e** | Linux LVM | LVM physical volume |
| **c** | W95 FAT32 (LBA) | Windows FAT32 |
| **7** | NTFS | Windows NTFS |
| **5** | Extended | Container for logical partitions (MBR only) |

---

## Mount Points

**Mount Point** = Directory where partition connects to filesystem

**Common mount points:**
*   `/` - Root filesystem
*   `/boot` - Boot files
*   `/home` - User home directories
*   `/var` - Variable data (logs, databases)
*   `/tmp` - Temporary files

**Example:**
```
/dev/sda1 mounted on /boot
/dev/sda2 → LVM → /
/dev/sda3 → LVM → /home
```

Files in `/home/user/documents/` are physically stored on `/dev/sda3`.

---

## Filesystem Components

After partitioning, partitions need:
1. **Filesystem** - Structure for organizing data (`mkfs`)
2. **Mount point** - Directory to access files (`mount`)
3. **Entry in /etc/fstab** - Automatic mounting at boot (optional)

---

## Review Questions

1. What is the difference between RAM and a hard disk?

2. What is swap space used for?

3. What are the limitations of MBR partition tables?

4. How many primary partitions can GPT support?

5. What device represents the second SATA/USB disk?

6. True or False: Partitioning a disk destroys all data on it.

7. What does partition type 8e represent?

8. What is a mount point?

---

## Quick Reference

```
Storage Types:
├── RAM        → Temporary (fast, expensive)
├── Disk       → Permanent (slower, cheaper)
└── Swap       → Overflow from RAM

Partition Tables:
├── MBR (dos)  → 4 primary, 2TB max, BIOS
└── GPT        → 128 primary, 9.4ZB max, UEFI

Device Naming:
├── /dev/sda   → First SCSI/SATA/USB disk
├── /dev/sdb   → Second disk
└── /dev/sda1  → First partition on first disk

Common Commands:
├── parted -l /dev/sda      → View partitions (GPT-aware)
├── fdisk -l /dev/sda       → View partitions (MBR-focused)
├── cat /proc/partitions    → Check kernel knowledge
└── partprobe /dev/sdb      → Refresh partition table

Partition Types:
├── 83 → Linux filesystem
├── 82 → Linux swap
└── 8e → Linux LVM
```
