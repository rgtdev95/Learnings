# Disk Partitioning

## Key Concepts

| Term | Definition |
|------|------------|
| **Partition** | Logical division of a physical disk |
| **Filesystem** | Structure for organizing files on a partition |
| **LVM** | Logical Volume Manager (flexible partitioning) |
| **RAID** | Redundant Array of Independent Disks |
| **MBR** | Master Boot Record (older partition table, 4 primary partitions max) |
| **GPT** | GUID Partition Table (newer, 128 partitions max) |

---

## Why Partition?

### Reasons for Multiple Partitions

1. **Multiple Operating Systems**
   *   Each OS needs separate partitions
   *   Boot loader lets you choose at startup

2. **Protection from Disk Space Issues**
   *   If `/home` fills up, `/var/log` still works
   *   Prevents one user from filling entire disk

3. **Easier Backups**
   *   Image backup of `/home` is faster than entire system
   *   Separate user data from OS

4. **Different Filesystem Types**
   *   Root (`/`) might use ext4
   *   Special directories might need different features

---

## Partition Types

### 1. Linux Partitions (Standard)

Create regular ext2/ext3/ext4/xfs filesystems.

**Use Cases:**
*   Root filesystem (`/`)
*   `/boot` partition
*   Any standard directory mount point

**Filesystem Types:**
*   **ext4:** Default for many distributions (balanced, journaling)
*   **xfs:** RHEL 8 default (good for large files, high performance)
*   **ext3:** Older journaling filesystem
*   **ext2:** Non-journaling (rarely used now)

### 2. LVM Partitions

**Logical Volume Manager** provides flexible disk management.

**Advantages:**
*   ✅ Resize partitions easily (grow/shrink)
*   ✅ Move partitions between physical disks
*   ✅ Combine multiple disks into one logical volume
*   ✅ Create snapshots for backups

**Typical Use:** `/home` as LVM to grow as user needs increase

**Structure:**
```
Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV)
```

### 3. RAID Partitions

Create redundant disk arrays for performance/reliability.

**Requirements:**
*   2+ RAID partitions
*   Preferably on separate physical disks

**RAID Levels:**
*   **RAID 0:** Striping (speed, no redundancy)
*   **RAID 1:** Mirroring (redundancy, one disk can fail)
*   **RAID 5:** Striping + parity (speed + redundancy)
*   **RAID 10:** Mirrored stripes (best performance + redundancy)

### 4. Swap Partitions

Extends virtual memory beyond physical RAM.

**Sizing:**
*   RAM ≤ 2GB: Swap = 2 × RAM
*   RAM > 2GB: Swap = RAM size + 2GB
*   Hibernation: Swap ≥ RAM size

**Modern Practice:** Many systems use swap files instead of partitions

---

## Partition Table Types

| Type | Max Partitions | Max Disk Size | Boot Mode |
|------|----------------|---------------|-----------|
| **MBR** | 4 primary (or 3 primary + 1 extended with 184 logical) | 2TB | BIOS |
| **GPT** | 128 primary | 9.4ZB | UEFI |

---

## Recommended Directory Partitions

### Critical Directories for Separate Partitions

| Directory | Recommended Size | Reason |
|-----------|------------------|--------|
| `/boot` | 1024 MiB | BIOS compatibility, boot files separately |
| `/` (root) | 6-20GB | Core system files |
| `/home` | Variable | User data, protect from root filesystem issues |
| `/var` | 2-10GB | Logs, web/FTP content, prevent DoS attacks |
| `/tmp` | 1-2GB | Temporary files, prevent disk fill |
| `/usr` | 10GB+ | Applications, can mount read-only for security |

### Example Partition Scheme (20GB Disk)

```
/boot     1GB     (ext4)
swap      4GB     (swap)
/         6GB     (xfs)
/var      2GB     (xfs)
/home     7GB     (xfs or LVM)
```

---

## Partitioning Tips

### Best Practices

1. **Install Windows First** (for dual-boot systems)
   *   Windows installation can damage Linux partitions
   *   Linux GRUB can boot both OSes

2. **Use Distribution's Partitioning Tools**
   *   `fdisk` for Linux partitions
   *   Windows `fdisk` for Windows partitions
   *   Avoid mixing tools

3. **Don't Need Many Partitions for Desktop**
   *   Minimum: `/` (root) + swap
   *   Simple setup: `/boot`, `/`, swap

4. **Servers Need More Partitions**
   *   Protect against attacks
   *   Isolate services
   *   Easier recovery from corruption

5. **Backup Before Partitioning**
   *   ALWAYS backup important data
   *   Partitioning can destroy data

6. **Use LVM for Flexibility**
   *   Easy to resize later
   *   Better than fixed partitions for growing needs

### Protection Benefits

| Benefit | How It Helps |
|---------|--------------|
| **Attack Protection** | DoS attacks filling `/var` won't crash system |
| **Corruption Isolation** | Corrupted `/home` doesn't break `/` |
| **Easier Backups** | Image `/home` without entire system |
| **Performance** | Different mount options per partition |

---

## Dual-Boot Resizing

### Prepare Windows Partition

1. **Backup everything**
2. **Defragment disk**
   *   My Computer → Right-click C: → Properties → Tools → Defragment Now
3. **Disable Windows swap file**
   *   Control Panel → System → Performance → Virtual Memory → Disable
4. **Defragment again**
5. **Re-enable swap file**

### Resize Tools

*   **GParted** (open-source, Linux)
*   **Acronis Disk Director** (commercial)
*   **Windows Disk Management** (built-in, limited)

---

## Storage Options During Installation

### Specialized Storage

*   **Firmware RAID:** Configured in BIOS, can boot OS
*   **Multipath Devices:** Multiple paths to storage (performance/redundancy)
*   **iSCSI:** Network-based storage (requires target IP, credentials)
*   **FCoE:** Fibre Channel over Ethernet (NIC connected to FCoE switch)

---

## Review Questions

1. What are three reasons to create multiple partitions?

2. What is the default filesystem type for RHEL 8?

3. What does LVM stand for?

4. Which partition type extends virtual memory?

5. How many primary partitions can an MBR partition table have?

6. Which directory should be on a separate partition to prevent boot issues?

7. True or False: `/home` is often created as an LVM logical volume.

8. What tool can resize Windows NTFS partitions from Linux?

---

## Quick Reference

```
Partition Types:
├── Linux       → ext4, xfs (standard filesystems)
├── LVM         → Flexible volumes (grow/shrink)
├── RAID        → Redundancy/performance
└── Swap        → Virtual memory extension

Common Layout:
/boot   1GB
swap    4GB (RAM-based sizing)
/       6-20GB
/var    2-10GB
/home   Remaining (or LVM)
```

