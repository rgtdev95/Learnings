# Part 5: Learning Linux Security Techniques

## Overview

Part 5 covers advanced security topics essential for protecting Linux systems. This section explores physical security, user management, cryptography, authentication modules, and SELinux mandatory access controls.

**Chapters:**
- Chapter 22: Understanding Basic Linux Security
- Chapter 23: Understanding Advanced Linux Security  
- Chapter 24: Enhancing Linux Security with SELinux

**Total Notes Files:** 15

---

## Chapter Summaries

### Chapter 22: Understanding Basic Linux Security

Covers fundamental security practices including physical security, user account management, password policies, filesystem permissions, monitoring, and intrusion detection.

| Topic | Notes File | Key Concepts |
|-------|-----------|--------------|
| Physical Security | [01-physical-security.md](../Notes/part-5/01-physical-security.md) | Server room security, disaster recovery, backup utilities (amanda, tar, rsync) |
| User Accounts | [02-user-account-security.md](../Notes/part-5/02-user-account-security.md) | sudo usage, account expiration, removal procedures |
| Passwords | [03-password-security.md](../Notes/part-5/03-password-security.md) | Strong passwords, aging policies, /etc/shadow, PAM cracklib |
| Filesystem | [04-filesystem-security.md](../Notes/part-5/04-filesystem-security.md) | SUID/SGID permissions, fstab mount options (nosuid, nodev, noexec) |
| Monitoring | [05-monitoring-security.md](../Notes/part-5/05-monitoring-security.md) | Log files, journalctl, auditd, John the Ripper, rpm verification |
| Intrusion Detection | [06-intrusion-detection.md](../Notes/part-5/06-intrusion-detection.md) | ClamAV, chkrootkit, aide, Kali Linux, penetration testing |

### Chapter 23: Understanding Advanced Linux Security

Explores cryptography fundamentals, encryption tools, digital signatures, and Pluggable Authentication Modules (PAM) for centralized authentication management.

| Topic | Notes File | Key Concepts |
|-------|-----------|--------------|
| Cryptography | [07-cryptography-basics.md](../Notes/part-5/07-cryptography-basics.md) | Hashing vs encryption, ciphers (AES, RSA), symmetric/asymmetric keys |
| GPG Encryption | [08-gpg-encryption.md](../Notes/part-5/08-gpg-encryption.md) | GNU Privacy Guard, key pairs, file encryption, key management |
| Digital Signatures | [09-digital-signatures.md](../Notes/part-5/09-digital-signatures.md) | Signature creation/verification, non-repudiation, authentication |
| Filesystem Encryption | [10-filesystem-encryption.md](../Notes/part-5/10-filesystem-encryption.md) | LUKS disk encryption, ecryptfs directory encryption |
| PAM | [11-pam-authentication.md](../Notes/part-5/11-pam-authentication.md) | PAM modules, contexts, control flags, resource limits, time restrictions |

### Chapter 24: Enhancing Linux Security with SELinux

Covers Security-Enhanced Linux (SELinux), implementing Mandatory Access Control (MAC) through security contexts, policies, and role-based access control.

| Topic | Notes File | Key Concepts |
|-------|-----------|--------------|
| SELinux Basics | [12-selinux-basics.md](../Notes/part-5/12-selinux-basics.md) | DAC vs MAC, operational modes, policy types (targeted, MLS) |
| Security Contexts | [13-selinux-contexts.md](../Notes/part-5/13-selinux-contexts.md) | User, role, type, level attributes; file/process contexts |
| Booleans & Policies | [14-selinux-booleans.md](../Notes/part-5/14-selinux-booleans.md) | Policy toggles, common booleans, policy modules |
| Troubleshooting | [15-selinux-troubleshooting.md](../Notes/part-5/15-selinux-troubleshooting.md) | AVC denials, audit logs, common problems, fixes |

---

## Learning Objectives

After completing Part 5, you should be able to:

**Basic Security:**
- Implement physical security measures and disaster recovery plans
- Manage user accounts securely with sudo and expiration policies
- Enforce strong password requirements and aging policies
- Secure filesystems with proper permissions and mount options
- Monitor system security through logs and auditing
- Detect intrusions using antivirus and rootkit scanners

**Advanced Security:**
- Understand cryptography fundamentals (hashing, encryption, ciphers)
- Use GPG for file encryption and digital signatures
- Encrypt filesystems and directories with LUKS and ecryptfs
- Configure PAM for centralized authentication management
- Implement resource limits and time restrictions with PAM

**SELinux:**
- Understand MAC vs DAC security models
- Configure SELinux modes (disabled, permissive, enforcing)
- Manage security contexts for users, files, and processes
- Use booleans to modify SELinux policies
- Troubleshoot AVC denials and fix common SELinux problems

---

## Key Commands Reference

### Basic Security Commands

```bash
# User Account Management
sudo command                    # Run command as root
usermod -e yyyy-mm-dd user      # Set account expiration
chage -l user                   # View password aging
userdel -r user                 # Delete user and home

# Password Management
passwd                          # Change password
chage -M 30 -m 5 -W 7 user     # Set password aging

# Filesystem Security
find / -perm -4000              # Find SUID files
find / -perm -2000              # Find SGID files

# Monitoring
journalctl -f                   # Follow system logs
auditctl -w /etc/passwd -p rwa  # Watch file
ausearch -f /etc/passwd         # Search audit logs
rpm -V package                  # Verify package integrity

# Intrusion Detection
chkrootkit                      # Scan for rootkits
aide -i                         # Initialize IDS database
aide -C                         # Check for changes
```

### Cryptography Commands

```bash
# Hashing
sha256sum file                  # Create SHA-256 hash

# GPG Symmetric Encryption
gpg2 -c -o encrypted.gpg file   # Encrypt file
gpg2 -d encrypted.gpg > file    # Decrypt file

# GPG Asymmetric Encryption
gpg2 --gen-key                  # Generate key pair
gpg2 --list-keys                # List public keys
gpg2 --export "Name" > key.pub  # Export public key
gpg2 --import key.pub           # Import public key
gpg2 --encrypt --recipient "Name" file  # Encrypt for recipient
gpg2 --decrypt file             # Decrypt with private key

# Digital Signatures
gpg2 --sign file                # Sign file
gpg2 --verify file              # Verify signature

# Filesystem Encryption
cryptsetup status device        # Check LUKS status
mount -t ecryptfs dir dir       # Mount encrypted directory
```

### PAM Commands

```bash
# View PAM configuration
cat /etc/pam.d/system-auth      # System authentication
cat /etc/security/limits.conf   # Resource limits
cat /etc/security/time.conf     # Time restrictions

# Check if PAM-aware
ldd /usr/bin/command | grep pam

# Manage users
semanage login -l               # View user mappings
```

### SELinux Commands

```bash
# Status and Mode
getenforce                      # Get current mode
sestatus                        # Detailed status
setenforce 0                    # Set permissive (temporary)
setenforce 1                    # Set enforcing (temporary)

# Security Contexts
ls -Z file                      # View file context
ps -eZ | grep process           # View process context
id                              # View your context

# Manage Contexts
chcon -t type file              # Change file type (temporary)
restorecon file                 # Restore default context
semanage fcontext -a -t type "path(/.*)?"  # Add persistent rule
semanage port -a -t type -p tcp port       # Add port label

# Booleans
getsebool -a                    # List all booleans
getsebool boolean               # View specific boolean
setsebool -P boolean on         # Set boolean (permanent)

# Troubleshooting
ausearch -m avc                 # Search for AVC denials
sealert -a /var/log/audit/audit.log  # Analyze denials
audit2allow -a                  # Generate policy from denials
```

---

## Important Files and Directories

### Basic Security

```
/etc/passwd                     # User account information
/etc/shadow                     # Encrypted passwords
/etc/group                      # Group information
/etc/sudoers                    # Sudo configuration
/etc/login.defs                 # Password aging defaults
/etc/fstab                      # Filesystem mount options
/var/log/secure                 # Security log
/var/log/audit/audit.log        # Audit log
```

### Cryptography & PAM

```
~/.gnupg/                       # GPG key storage
/etc/crypttab                   # Encrypted volume mappings
/etc/pam.d/                     # PAM configuration files
/etc/security/limits.conf       # Resource limits
/etc/security/time.conf         # Time restrictions
/usr/lib64/security/            # PAM modules
```

### SELinux

```
/etc/selinux/config             # SELinux configuration
/etc/selinux/targeted/          # Targeted policy files
/var/log/audit/audit.log        # AVC denials
/sys/fs/selinux/                # SELinux filesystem
```

---

## Security Best Practices

**Physical Security:**
- Lock server rooms with access controls
- Implement disaster recovery plans
- Regular offsite backups (3-2-1 rule)
- Test restore procedures

**Account Security:**
- One user per account
- Use sudo instead of root login
- Set account expiration for temporary users
- Regular account audits

**Password Security:**
- Minimum 15-25 characters
- Mix of uppercase, lowercase, numbers, symbols
- Password aging (30-90 days)
- Enforce with PAM cracklib

**Filesystem Security:**
- Minimize SUID/SGID files
- Use mount options (nosuid, nodev, noexec)
- Regular integrity checks
- Proper file permissions

**Monitoring:**
- Review logs daily
- Use auditd for file watching
- Regular vulnerability scans
- Intrusion detection systems

**Encryption:**
- Encrypt sensitive data
- Use strong ciphers (AES-256)
- Protect private keys
- Encrypt before cloud upload

**SELinux:**
- Start with permissive mode
- Review AVC denials
- Use booleans over custom policies
- Test changes in non-production

---

## Related Resources

**Documentation:**
- Man pages: `man selinux`, `man pam`, `man gpg2`
- Red Hat Security Guide
- Fedora Security Guide
- SELinux Project Wiki

**Tools:**
- Kali Linux (penetration testing)
- OpenVAS (vulnerability scanner)
- Nmap (network scanner)
- Wireshark (network analysis)

**Standards:**
- CIS Benchmarks
- NIST Security Guidelines
- PCI-DSS Compliance
- HIPAA Security Rules

---

## Quiz

Test your knowledge with the [Part 5 Quiz](../../Quiz/Part-5/part-5-quiz.md) covering all three chapters.

---

**Part 5 Complete!** You now have a comprehensive understanding of Linux security techniques, from basic physical security to advanced SELinux mandatory access controls.
