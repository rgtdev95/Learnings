# Filesystem and File Encryption

## Encrypting Filesystem at Installation

### LUKS (Linux Unified Key Setup)

**What is LUKS:**
*   Free and Open Source Software (FOSS)
*   Standard for hard disk encryption
*   Symmetric key cryptography
*   Website: https://gitlab.com/cryptsetup/cryptsetup

**Installation encryption:**
*   Available during Linux installation
*   Encrypts root partition
*   Password-protected
*   Minimum 8 characters

**At boot:**
*   Prompted for encryption password
*   Required to decrypt and mount filesystem
*   Protects against disk theft

---

### Checking Encrypted Volumes

**List logical volumes:**
```bash
lvs -o devices
```

**Example output:**
```
Devices
/dev/mapper/luks-b099fbbe-0e56-425f-91a6-44f129db9f4b(56)
/dev/mapper/luks-b099fbbe-0e56-425f-91a6-44f129db9f4b(0)
```

**LUKS indicator:**
*   Device names start with `luks-`
*   Indicates LUKS encryption used

**Ubuntu note:**
```bash
# Install LVM tools
apt-get install lvm2
```

---

### /etc/crypttab File

**Purpose:**
*   Triggers password capture at boot
*   Decrypts /etc/fstab entries
*   Maps encrypted devices

**View crypttab:**
```bash
cat /etc/crypttab
```

**Example:**
```
luks-b099fbbe-0e56-425f-91a6-44f129db9f4b UUID=b099fbbe-0e56-425f-91a6-44f129db9f4b none
```

---

### cryptsetup Command

**Check encryption status:**
```bash
cryptsetup status luks-DEVICE-NAME
```

**Example output:**
```
/dev/mapper/luks-b099fbbe-0e56-425f-91a6-44f129db9f4b is active and is in use.
  type:    LUKS1
  cipher:  aes-xts-plain64
  keysize: 512 bits
  device:  /dev/sda3
  offset:  4096 sectors
  size:    493819904 sectors
  mode:    read/write
```

**Information shown:**
*   Type (LUKS1, LUKS2)
*   Cipher algorithm
*   Key size
*   Underlying device
*   Status

---

## Encrypting Directories with ecryptfs

### What is ecryptfs

**Definition:**
*   POSIX-compliant utility
*   Not a filesystem type
*   Encryption layer on top of filesystem
*   Works with any filesystem

**Installation:**
```bash
# Fedora
dnf install ecryptfs-utils

# Ubuntu
apt-get install ecryptfs-utils
```

**Common typo:**
*   Correct: `ecryptfs`
*   Wrong: `encryptfs` (no 'n')

---

### Encrypting a Directory

**Preparation:**
*   Directory must be empty
*   Move existing files elsewhere
*   Cannot access files while encrypted

**Mount encrypted directory:**
```bash
mount -t ecryptfs /home/johndoe/Secret /home/johndoe/Secret
```

**Interactive prompts:**

**1. Key type:**
```
Select key type to use for newly created files:
 1) tspi
 2) passphrase
 3) pkcs11-helper
 4) openssl
Selection: 2
```

**2. Passphrase:**
```
Passphrase: **********
```

**3. Cipher:**
```
Select cipher:
 1) aes: blocksize = 16; min keysize = 16; max keysize = 32 (loaded)
 2) blowfish: blocksize = 16; min keysize = 16; max keysize = 56 (not loaded)
 ...
Selection [aes]: 1
```

**4. Key bytes:**
```
Select key bytes:
 1) 16
 2) 32
 3) 24
Selection [16]: 16
```

**5. Plaintext passthrough:**
```
Enable plaintext passthrough (y/n) [n]: n
```

**6. Filename encryption:**
```
Enable filename encryption (y/n) [n]: n
```

**Important:**
*   Write down selections
*   Need exact same selections to remount
*   Cannot access if you forget

---

### Using Encrypted Directory

**Verify mounted:**
```bash
mount | grep /home/johndoe/Secret
```

**Output:**
```
/home/johndoe/Secret on /home/johndoe/Secret type ecryptfs
(rw,relatime,ecryptfs_sig=70993b8d49610e67,ecryptfs_cipher=aes,
ecryptfs_key_bytes=16,ecryptfs_unlink_sigs)
```

**Copy files:**
```bash
cp my_secret_file Secret/
```

**Read files:**
*   Files automatically decrypted
*   User can read normally
*   Root can read normally

**Example:**
```bash
cat /home/johndoe/Secret/my_secret_file
# Output: Shh... It's a secret.
```

---

### Unmounting Encrypted Directory

**Unmount:**
```bash
umount /home/johndoe/Secret
```

**After unmount:**
*   Files become gibberish
*   Cannot be read
*   Even root cannot decrypt
*   Must remount to access

**Example:**
```bash
cat /home/johndoe/Secret/my_secret_file
# Output: Encrypted gibberish
```

---

### Non-Root User Encryption

**Setup private mount:**
```bash
ecryptfs-setup-private
```

**Mount as non-root:**
```bash
ecryptfs-mount-private
```

**Benefits:**
*   No root privileges needed
*   User-specific encryption
*   Personal encrypted directory

---

## Cloud Storage Encryption

**Problem:**
*   Some cloud providers (e.g., Dropbox)
*   Don't encrypt until received
*   Provider has decryption keys
*   Data vulnerable

**Solution:**
*   Encrypt files before upload
*   Use GPG or ecryptfs
*   Extra layer of protection
*   Provider cannot decrypt

---

## Review Questions

1. What is LUKS?

2. What file triggers password capture for encrypted volumes at boot?

3. What command checks encryption status?

4. What is ecryptfs?

5. How do you mount an ecryptfs directory?

6. What happens to files when ecryptfs directory is unmounted?

7. True or False: ecryptfs is a filesystem type.

8. Why should you encrypt files before uploading to cloud storage?

---

## Quick Reference

```
LUKS:
├── Linux Unified Key Setup
├── Disk encryption standard
├── Symmetric key cryptography
└── Set during installation

Check Encrypted Volumes:
├── lvs -o devices                → List volumes
├── cat /etc/crypttab             → View encrypted mappings
└── cryptsetup status luks-NAME   → Check status

cryptsetup Output:
├── type    → LUKS1, LUKS2
├── cipher  → Encryption algorithm
├── keysize → Key size in bits
├── device  → Underlying device
└── mode    → read/write status

ecryptfs:
├── Install: dnf install ecryptfs-utils
├── Encryption layer on filesystem
├── Works with any filesystem
└── NOT a filesystem type

Mount ecryptfs:
└── mount -t ecryptfs /dir /dir

Interactive Selections:
├── Key type: passphrase
├── Passphrase: (your password)
├── Cipher: aes
├── Key bytes: 16, 32, or 24
├── Plaintext passthrough: n
└── Filename encryption: n

Using ecryptfs:
├── Verify: mount | grep /dir
├── Copy files: cp file /dir/
├── Files auto-decrypted when mounted
└── Unmount: umount /dir

After Unmount:
├── Files become gibberish
├── Cannot be read
└── Must remount to access

Non-Root ecryptfs:
├── Setup: ecryptfs-setup-private
└── Mount: ecryptfs-mount-private

Cloud Storage:
├── Problem: Provider has keys
├── Solution: Encrypt before upload
└── Tools: GPG, ecryptfs

Installation Encryption:
├── Available during install
├── Encrypts root partition
├── Password at boot
└── Minimum 8 characters

Best Practices:
├── Write down ecryptfs selections
├── Test encryption/decryption
├── Backup encryption keys
├── Use strong passphrases
└── Encrypt before cloud upload
```
