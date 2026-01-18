# LVM (Logical Volume Manager)

## LVM Overview

**Problem:** Traditional partitions are inflexible
*   Can't easily resize
*   Can't span multiple disks
*   Difficult to manage growing storage needs

**Solution:** LVM provides flexible storage management

---

## LVM Architecture

```
Physical Volumes (PV) → Volume Groups (VG) → Logical Volumes (LV)
```

### Three Layers

**1. Physical Volume (PV):**
*   Disk partition (type 8e)
*   Entire disk
*   Network storage

**2. Volume Group (VG):**
*   Pool of storage from multiple PVs
*   Divided into Physical Extents (PE), typically 4MB

**3. Logical Volume (LV):**
*   "Virtual partition" carved from VG
*   Can be formatted and mounted like regular partition
*   Easily resized

---

## LVM Visualization

```
Disks:
/dev/sda2 (150GB) → PV
/dev/sdb1 (100GB) → PV

Volume Group (vg_abc): 250GB
  ├── lv_root (50GB) → mounted on /
  ├── lv_home (150GB) → mounted on /home
  └── lv_swap (6GB) → swap
  Free space: 44GB
```

---

## LVM Commands Overview

| Component | Create | Display | Extend | Remove |
|-----------|--------|---------|--------|--------|
| **Physical Volume** | `pvcreate` | `pvdisplay` | N/A | `pvremove` |
| **Volume Group** | `vgcreate` | `vgdisplay` | `vgextend` | `vgremove` |
| **Logical Volume** | `lvcreate` | `lvdisplay` | `lvextend` | `lvremove` |

---

## Checking Existing LVM

### View Physical Volumes

```bash
sudo pvdisplay /dev/sda2
```

**Shows:**
*   PV name
*   VG name it belongs to
*   PV size
*   PE size (typically 4MB)
*   Total/free PEs

### View Volume Groups

```bash
sudo vgdisplay vg_abc
```

**Shows:**
*   VG name
*   VG size
*   PE size
*   Total/allocated/free PEs

### View Logical Volumes

```bash
sudo lvdisplay vg_abc
sudo lvdisplay /dev/vg_abc/lv_root
```

**Shows:**
*   LV name (e.g., `/dev/vg_abc/lv_root`)
*   VG name
*   LV size
*   Status

---

## Creating LVM From Scratch

### Step 1: Create Partition

```bash
# Create partition type 8e (Linux LVM)
sudo fdisk /dev/sdb
# ... create /dev/sdb6 as type 8e
```

### Step 2: Create Physical Volume

```bash
sudo pvcreate /dev/sdb6
```

### Step 3: Create Volume Group

```bash
sudo vgcreate myvg0 /dev/sdb6
```

**Check:**
```bash
sudo vgdisplay myvg0
```

### Step 4: Create Logical Volume

```bash
# Create 1GB LV named "music"
sudo lvcreate -n music -L 1G myvg0
```

**Device created:** `/dev/mapper/myvg0-music`

**Alternative paths:**
*   `/dev/myvg0/music`
*   `/dev/mapper/myvg0-music`

### Step 5: Create Filesystem

```bash
sudo mkfs -t ext4 /dev/mapper/myvg0-music
```

### Step 6: Mount

```bash
sudo mkdir /mnt/mymusic
sudo mount /dev/mapper/myvg0-music /mnt/mymusic
df -h /mnt/mymusic
```

### Step 7: Make Permanent (Optional)

Add to `/etc/fstab`:
```
/dev/mapper/myvg0-music /mnt/mymusic ext4 defaults 1 2
```

---

## Growing Logical Volumes

**Advantage:** Grow LVs without unmounting!

### Example: Grow from 1GB to 2GB

**1. Check available space:**
```bash
sudo vgdisplay myvg0 | grep "Free"
```

**2. Extend logical volume:**
```bash
sudolvextend -L +1G /dev/mapper/myvg0-music
```

**3. Resize filesystem:**

**For ext4:**
```bash
sudo resize2fs /dev/mapper/myvg0-music
```

**For xfs:**
```bash
sudo xfs_growfs /mnt/mymusic
```

**4. Verify:**
```bash
df -h /mnt/mymusic
```

---

## LVM Benefits

✅ **Flexibility:** Resize volumes easily
✅ **Spanning:** Combine multiple disks
✅ **Snapshots:** Create point-in-time copies
✅ **No downtime:** Grow while mounted
✅ **Easy migration:** Move data between PVs

---

## LVM Device Names

**Logical volumes have multiple names:**

```
/dev/vg_abc/lv_root       → Symbolic link
/dev/mapper/vg_abc-lv_root → Actual device
```

Both work interchangeably!

---

## Common LVM Operations

### Add Disk to Volume Group

```bash
# Create PV on new disk
sudo pvcreate /dev/sdc1

# Add to existing VG
sudo vgextend vg_abc /dev/sdc1
```

### Create Snapshot

```bash
sudo lvcreate -L 1G -s -n lv_root_snap /dev/vg_abc/lv_root
```

### Remove Logical Volume

```bash
# Unmount first!
sudo umount /mnt/mymusic

# Remove LV
sudo lvremove /dev/mapper/myvg0-music
```

---

## Review Questions

1. What are the three components of LVM?

2. What command creates a physical volume?

3. What is a Physical Extent (PE)?

4. How do you create a 5GB logical volume named "data" in VG "myvg"?

5. True or False: You must unmount an LV to grow it.

6. Which command extends a logical volume?

7. What partition type should be used for LVM?

8. Where are LVM device mapper files located?

---

## Quick Reference

```
LVM Hierarchy:
Physical Volume → Volume Group → Logical Volume
(PV)              (VG)            (LV)

Create LVM:
├── pvcreate /dev/sdb6       → Create PV
├── vgcreate myvg /dev/sdb6  → Create VG
├── lvcreate -n data -L 5G myvg → Create LV
├── mkfs -t ext4 /dev/mapper/myvg-data → Format
└── mount /dev/mapper/myvg-data /mnt → Mount

Grow LVM:
├── vgdisplay myvg           → Check free space
├── lvextend -L +2G /dev/mapper/myvg-data → Extend LV
└── resize2fs /dev/mapper/myvg-data → Resize filesystem

Display:
├── pvdisplay    → Show PVs
├── vgdisplay    → Show VGs
└── lvdisplay    → Show LVs

Device Names:
├── /dev/vg_abc/lv_root
└── /dev/mapper/vg_abc-lv_root
    (Both are equivalent)
```
