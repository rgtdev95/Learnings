# Physical Security and Disaster Recovery

## Physical Security Basics

**First line of defense:** Lock on the server room door

**Why it matters:**
*   Access to physical server = access to all data
*   No security software can protect against physical access
*   Simple concept, often ignored

---

## Server Room Security

**Basic requirements:**

**Lock and alarm:**
*   Lock on server room door
*   Security alarm system
*   Prevents unauthorized entry

**Access controls:**
*   Card key entry system
*   Biometric scanners
*   Track who accessed room and when
*   Allow only authorized personnel

**Signage:**
*   "No unauthorized access allowed"
*   Clear warning on door
*   Legal protection

**Access policies:**
*   Define who can access
*   Define when access allowed
*   Cleaning crew procedures
*   Administrator schedules
*   Visitor policies

---

## Environmental Controls

**Fire suppression:**
*   Appropriate fire suppression systems
*   Halon or clean agent systems
*   Avoid water-based systems near electronics
*   Regular testing and maintenance

**Ventilation:**
*   Proper cooling systems
*   Temperature monitoring
*   Humidity control
*   Prevent overheating
*   Extend hardware life

**Power:**
*   UPS (Uninterruptible Power Supply)
*   Backup generators
*   Surge protection
*   Clean power supply

---

## Disaster Recovery Planning

**Key components:**

**What to backup:**
*   Critical data files
*   Configuration files
*   User data
*   System state
*   Application data
*   Databases

**Where to store:**
*   Offsite location
*   Secure facility
*   Fireproof safe
*   Cloud storage
*   Geographic diversity

**How long to maintain:**
*   Daily backups: 1 week
*   Weekly backups: 1 month
*   Monthly backups: 1 year
*   Yearly backups: 7+ years
*   Depends on compliance requirements

**Media rotation:**
*   Grandfather-Father-Son (GFS)
*   Tower of Hanoi
*   First In First Out (FIFO)
*   Regular rotation schedule

---

## Backup Utilities

### amanda (Advanced Maryland Automatic Network Disk Archiver)

**Features:**
*   Network backup solution
*   Backs up multiple machines
*   Flexible scheduling
*   Can backup Windows systems
*   Not installed by default

**Installation:**
```bash
dnf install amanda
apt-get install amanda-server amanda-client
```

**More info:** amanda.org

### cpio

**Features:**
*   Copy files to/from archives
*   Typically pre-installed
*   Works with find command

**Basic usage:**
```bash
# Create archive
find . -print | cpio -o > archive.cpio

# Extract archive
cpio -i < archive.cpio

# List contents
cpio -t < archive.cpio
```

### dump/restore

**Features:**
*   Filesystem-level backup
*   Incremental backups
*   Typically pre-installed
*   Works on ext2/ext3/ext4

**Basic usage:**
```bash
# Full backup (level 0)
dump -0u -f /backup/home.dump /home

# Incremental backup (level 1)
dump -1u -f /backup/home-inc.dump /home

# Restore
restore -rf /backup/home.dump
```

### tar

**Features:**
*   Tape archive utility
*   Most common backup tool
*   Pre-installed
*   Compression support

**Basic usage:**
```bash
# Create archive
tar -czf backup.tar.gz /home

# Extract archive
tar -xzf backup.tar.gz

# List contents
tar -tzf backup.tar.gz

# Incremental backup
tar -czf backup.tar.gz --listed-incremental=snapshot.file /home
```

### rsync

**Features:**
*   Synchronize files/directories
*   Network transfer
*   Only transfers differences
*   Efficient for large datasets
*   Excellent for automated backups

**Basic usage:**
```bash
# Local sync
rsync -av /source/ /destination/

# Remote sync (push)
rsync -av /local/ user@remote:/remote/

# Remote sync (pull)
rsync -av user@remote:/remote/ /local/

# With compression
rsync -avz /source/ user@remote:/destination/

# Delete files not in source
rsync -av --delete /source/ /destination/
```

**Automated backup with cron:**
```bash
# Daily backup at 2 AM
0 2 * * * rsync -avz /data/ backup@server:/backups/$(date +\%Y-\%m-\%d)/
```

---

## Access Control Matrix

**What to include:**

**Backup data:**
*   Who can access backups
*   Who can restore data
*   Encryption requirements

**Backup media:**
*   Physical media security
*   Transport procedures
*   Storage location access

**Backup software:**
*   Who can configure
*   Who can run backups
*   Who can modify schedules

---

## Backup Best Practices

**Multiple copies:**
*   3-2-1 rule: 3 copies, 2 different media, 1 offsite
*   Protect against single point of failure

**Test restores:**
*   Regular restore testing
*   Verify backup integrity
*   Practice recovery procedures
*   Document recovery time

**Encryption:**
*   Encrypt sensitive backups
*   Secure encryption keys
*   Separate key storage

**Documentation:**
*   Backup procedures
*   Restore procedures
*   Contact information
*   Recovery time objectives (RTO)
*   Recovery point objectives (RPO)

---

## Review Questions

1. What is the first line of defense for server security?

2. Name three components of server room physical security.

3. What are the four key components of a disaster recovery plan?

4. What backup utility is best for network backups of multiple machines?

5. What does rsync do differently than tar?

6. What is the 3-2-1 backup rule?

7. True or False: Backups should be stored in the same building as the server.

8. What command creates a compressed tar archive?

---

## Quick Reference

```
Physical Security:
├── Lock on door
├── Access controls (card key)
├── Security alarm
├── Signage
└── Access policies

Environmental:
├── Fire suppression
├── Proper ventilation
├── Temperature control
└── UPS/backup power

Disaster Recovery:
├── What to backup
├── Where to store (offsite)
├── How long to maintain
└── Media rotation schedule

Backup Utilities:
├── amanda   → Network backup, multiple machines
├── cpio     → Archive utility
├── dump     → Filesystem-level backup
├── tar      → Tape archive, compression
└── rsync    → Sync files, efficient transfers

tar Commands:
├── tar -czf backup.tar.gz /dir  → Create compressed
├── tar -xzf backup.tar.gz       → Extract
└── tar -tzf backup.tar.gz       → List contents

rsync Commands:
├── rsync -av /src/ /dst/              → Local sync
├── rsync -av /src/ user@host:/dst/    → Remote sync
├── rsync -avz /src/ user@host:/dst/   → With compression
└── rsync -av --delete /src/ /dst/     → Delete extra files

3-2-1 Backup Rule:
├── 3 copies of data
├── 2 different media types
└── 1 copy offsite

Best Practices:
├── Test restores regularly
├── Encrypt sensitive backups
├── Document procedures
├── Store offsite
└── Rotate media
```
