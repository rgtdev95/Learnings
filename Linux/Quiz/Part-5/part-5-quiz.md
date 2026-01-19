# Part 5: Learning Linux Security Techniques - Quiz

## Overview

This quiz covers Part 5 of the Linux Bible (10th Edition): "Learning Linux Security Techniques"

**Chapters Covered:**
- Chapter 22: Understanding Basic Linux Security
- Chapter 23: Understanding Advanced Linux Security
- Chapter 24: Enhancing Linux Security with SELinux

**Total Questions:** 75

---

## Chapter 22: Understanding Basic Linux Security (25 questions)

### Physical Security & Disaster Recovery

1. What is considered the first line of defense for server security?
   - A) Firewall
   - B) Lock on the server room door
   - C) Antivirus software
   - D) SELinux

2. What is the 3-2-1 backup rule?
   - A) 3 backups, 2 locations, 1 administrator
   - B) 3 copies, 2 different media, 1 offsite
   - C) 3 servers, 2 networks, 1 firewall
   - D) 3 passwords, 2 factors, 1 key

3. Which backup utility is best for network backups of multiple machines?
   - A) tar
   - B) cpio
   - C) amanda
   - D) dump

4. What does rsync do differently than tar for backups?
   - A) Creates compressed archives
   - B) Only transfers differences
   - C) Encrypts data
   - D) Supports tape drives

5. Where should backup media be stored for disaster recovery?
   - A) Same building as server
   - B) Server room
   - C) Offsite location
   - D) Administrator's desk

### User Account Security

6. Why should each person have their own user account?
   - A) Easier to remember passwords
   - B) Accountability and audit trails
   - C) Saves disk space
   - D) Faster login times

7. What command sets an account expiration date?
   - A) `passwd -e`
   - B) `usermod -e yyyy-mm-dd username`
   - C) `chage -E username`
   - D) `expire username`

8. What file contains hashed passwords on modern Linux systems?
   - A) /etc/passwd
   - B) /etc/shadow
   - C) /etc/security
   - D) /etc/login.defs

9. What are the benefits of using sudo instead of sharing the root password?
   - A) Faster execution
   - B) Logging and accountability
   - C) Less memory usage
   - D) Better performance

10. What command finds all files owned by a specific user?
    - A) `ls -l username`
    - B) `find / -user username`
    - C) `locate username`
    - D) `grep username /etc/passwd`

### Password Security

11. What is the recommended minimum password length for strong security?
    - A) 8 characters
    - B) 10 characters
    - C) 15-25 characters
    - D) 30 characters

12. What file contains default password aging settings for new accounts?
    - A) /etc/passwd
    - B) /etc/shadow
    - C) /etc/login.defs
    - D) /etc/security/pwquality.conf

13. What command sets password aging for existing accounts?
    - A) passwd
    - B) chage
    - C) usermod
    - D) pwconv

14. What PAM module enforces password strength requirements?
    - A) pam_unix
    - B) pam_cracklib
    - C) pam_limits
    - D) pam_time

15. What does the `-I` option in chage do?
    - A) Initialize password
    - B) Set inactive days after expiration before account locks
    - C) Ignore password rules
    - D) Import password settings

### Filesystem Security

16. What does the SUID permission do?
    - A) Allows user to run file as file's owner
    - B) Prevents file deletion
    - C) Encrypts the file
    - D) Makes file immutable

17. What command finds all files with SUID or SGID set?
    - A) `find / -perm 6000`
    - B) `find / -perm /6000`
    - C) `ls -l / | grep s`
    - D) `locate suid`

18. What should the permissions be for /etc/shadow?
    - A) 644 (rw-r--r--)
    - B) 600 (rw-------)
    - C) 000 (----------)
    - D) 400 (r--------)

19. What mount option prevents SUID/SGID programs from running?
    - A) noexec
    - B) nosuid
    - C) nodev
    - D) ro

20. Which partitions should have the noexec mount option?
    - A) /boot only
    - B) /home and /tmp
    - C) /usr only
    - D) All partitions

### Monitoring & Intrusion Detection

21. What command views kernel messages from systemd journal?
    - A) `dmesg`
    - B) `journalctl -k`
    - C) `cat /var/log/kern.log`
    - D) `tail /var/log/messages`

22. What tool tests password strength by attempting to crack them?
    - A) passwd
    - B) chage
    - C) John the Ripper
    - D) pwgen

23. What command verifies RPM package integrity?
    - A) `rpm -q`
    - B) `rpm -V`
    - C) `rpm -i`
    - D) `rpm --check`

24. What is a rootkit?
    - A) Root password cracker
    - B) Malicious program collection that hides itself and maintains root access
    - C) Backup utility
    - D) Filesystem checker

25. What tool is used for penetration testing on Linux?
    - A) Windows Defender
    - B) Kali Linux
    - C) Ubuntu Desktop
    - D) Fedora Workstation

---

## Chapter 23: Understanding Advanced Linux Security (25 questions)

### Cryptography Basics

26. What is the difference between hashing and encryption?
    - A) Hashing is reversible, encryption is not
    - B) Hashing is one-way, encryption is reversible
    - C) They are the same
    - D) Hashing is faster

27. What command creates a SHA-256 hash?
    - A) `md5sum`
    - B) `sha1sum`
    - C) `sha256sum`
    - D) `hash256`

28. Why is DES no longer considered secure?
    - A) Too slow
    - B) 56-bit key size is too small
    - C) Not open source
    - D) Incompatible with Linux

29. What's the difference between symmetric and asymmetric cryptography?
    - A) Symmetric uses one key, asymmetric uses two keys
    - B) Symmetric is slower
    - C) Asymmetric uses one key
    - D) No difference

30. Which cipher is most popular for asymmetric cryptography?
    - A) AES
    - B) DES
    - C) RSA
    - D) Blowfish

### GPG Encryption

31. What command generates a GPG key pair?
    - A) `gpg2 --create-key`
    - B) `gpg2 --gen-key`
    - C) `gpg2 --new-key`
    - D) `gpg2 --make-key`

32. How do you encrypt a file with symmetric encryption using GPG?
    - A) `gpg2 -e file`
    - B) `gpg2 -c file`
    - C) `gpg2 --symmetric file`
    - D) `gpg2 -s file`

33. What protects your private key?
    - A) Public key
    - B) Passphrase
    - C) Username
    - D) Root password

34. How do you export your public key?
    - A) `gpg2 --export "Name" > file.pub`
    - B) `gpg2 --send-key "Name"`
    - C) `gpg2 --public-key > file`
    - D) `gpg2 -e --key "Name"`

35. Can you share your private key with others?
    - A) Yes, it's designed to be shared
    - B) No, it must be kept secret
    - C) Only with trusted users
    - D) Only encrypted

### Digital Signatures

36. What is a digital signature?
    - A) Scanned image of your signature
    - B) Electronic authentication token
    - C) Password hash
    - D) Public key

37. What are the steps to create a digital signature?
    - A) Encrypt file with public key
    - B) Create hash, encrypt hash with private key
    - C) Create hash, encrypt hash with public key
    - D) Encrypt file with private key

38. What command signs a file with GPG?
    - A) `gpg2 --encrypt file`
    - B) `gpg2 --sign file`
    - C) `gpg2 --signature file`
    - D) `gpg2 -s file`

39. What's the difference between --sign and --clearsign?
    - A) No difference
    - B) --clearsign leaves message readable
    - C) --sign is faster
    - D) --clearsign encrypts

40. What is a detached signature?
    - A) Signature in separate file
    - B) Signature removed
    - C) Invalid signature
    - D) Encrypted signature

### Filesystem Encryption

41. What is LUKS?
    - A) Linux User Key System
    - B) Linux Unified Key Setup
    - C) Linux Universal Key Storage
    - D) Linux Utility Key Service

42. What file triggers password capture for encrypted volumes at boot?
    - A) /etc/fstab
    - B) /etc/crypttab
    - C) /etc/shadow
    - D) /etc/security

43. Is ecryptfs a filesystem type?
    - A) Yes
    - B) No, it's an encryption layer
    - C) Only on Ubuntu
    - D) Only on RHEL

44. What happens to files when an ecryptfs directory is unmounted?
    - A) Files are deleted
    - B) Files become gibberish/unreadable
    - C) Files remain readable
    - D) Files are compressed

45. Why should you encrypt files before uploading to cloud storage?
    - A) Faster upload
    - B) Provider may have decryption keys
    - C) Required by law
    - D) Saves bandwidth

### PAM

46. What does PAM stand for?
    - A) Password Authentication Module
    - B) Pluggable Authentication Modules
    - C) Process Access Manager
    - D) Privilege Administration Module

47. How do you check if an application is PAM-aware?
    - A) `ldd /path/to/command | grep pam`
    - B) `file /path/to/command`
    - C) `which command`
    - D) `rpm -q command`

48. What are the four PAM contexts?
    - A) user, group, file, process
    - B) auth, account, password, session
    - C) read, write, execute, delete
    - D) allow, deny, log, ignore

49. What's the difference between required and requisite control flags?
    - A) No difference
    - B) requisite fails immediately, required continues stack
    - C) required is stronger
    - D) requisite is optional

50. What PAM module enforces resource limits?
    - A) pam_unix
    - B) pam_limits
    - C) pam_cracklib
    - D) pam_time

---

## Chapter 24: Enhancing Linux Security with SELinux (25 questions)

### SELinux Basics

51. What does SELinux stand for?
    - A) Secure Enhanced Linux
    - B) Security Enhanced Linux
    - C) System Enhanced Linux
    - D) Service Enhanced Linux

52. What's the difference between DAC and MAC?
    - A) DAC is newer
    - B) MAC is mandatory, DAC is discretionary
    - C) No difference
    - D) DAC is stronger

53. What are the three SELinux operational modes?
    - A) on, off, test
    - B) disabled, permissive, enforcing
    - C) low, medium, high
    - D) basic, advanced, expert

54. What command shows the current SELinux mode?
    - A) `selinux`
    - B) `getenforce`
    - C) `semode`
    - D) `sestatus --mode`

55. What's the default SELinux policy type?
    - A) strict
    - B) targeted
    - C) mls
    - D) minimum

56. What does permissive mode do?
    - A) Disables SELinux
    - B) Logs violations without blocking
    - C) Enforces all policies
    - D) Allows all access

57. Does SELinux replace traditional Linux security?
    - A) Yes, completely
    - B) No, it's an additional layer
    - C) Only on RHEL
    - D) Only for network services

58. What happens when you switch from disabled to enforcing mode?
    - A) Nothing
    - B) Filesystem relabel occurs
    - C) System crashes
    - D) All files deleted

### Security Contexts

59. What are the four attributes of a security context?
    - A) owner, group, permissions, type
    - B) user, role, type, level
    - C) read, write, execute, delete
    - D) allow, deny, log, audit

60. What command shows your security context?
    - A) `whoami`
    - B) `id`
    - C) `secon`
    - D) `getcontext`

61. How do you view a file's security context?
    - A) `ls -l file`
    - B) `ls -Z file`
    - C) `file -Z file`
    - D) `stat file`

62. What does the `-Z` option do?
    - A) Compress output
    - B) Show SELinux security context
    - C) Zoom view
    - D) Zero output

63. What's the difference between `chcon` and `semanage fcontext`?
    - A) No difference
    - B) chcon is temporary, semanage is persistent
    - C) chcon is faster
    - D) semanage is deprecated

64. What command restores a file's default context?
    - A) `chcon --default file`
    - B) `restorecon file`
    - C) `semanage restore file`
    - D) `setcon file`

65. What does the `_t` suffix indicate?
    - A) Temporary
    - B) Type attribute
    - C) Test mode
    - D) Trusted

### Booleans & Policy Management

66. What is an SELinux Boolean?
    - A) True/false variable
    - B) Toggle switch for policy rules
    - C) Binary file
    - D) Backup setting

67. What command lists all Booleans?
    - A) `listbool`
    - B) `getsebool -a`
    - C) `semanage boolean -l`
    - D) `showbool`

68. How do you make a Boolean change permanent?
    - A) `setsebool boolean on`
    - B) `setsebool -P boolean on`
    - C) `semanage boolean --permanent on`
    - D) Edit /etc/selinux/config

69. What command lists policy modules?
    - A) `semodule -l`
    - B) `listmodules`
    - C) `semanage module -l`
    - D) `showmodules`

70. Does changing Booleans require a system reboot?
    - A) Yes, always
    - B) No
    - C) Only for network services
    - D) Only in enforcing mode

### Troubleshooting

71. What is an AVC denial?
    - A) Access Vector Cache denial
    - B) Advanced Virus Check denial
    - C) Automatic Verification Check denial
    - D) Audit Vector Control denial

72. Where are AVC denials logged if auditd is running?
    - A) /var/log/messages
    - B) /var/log/audit/audit.log
    - C) /var/log/selinux
    - D) /var/log/secure

73. What command searches for AVC denials in audit log?
    - A) `grep AVC /var/log/audit/audit.log`
    - B) `ausearch -m avc`
    - C) `journalctl --avc`
    - D) `sealert --search`

74. What should you always check before troubleshooting SELinux?
    - A) Network connectivity
    - B) Traditional DAC permissions
    - C) Disk space
    - D) CPU usage

75. What command temporarily disables dontaudit rules?
    - A) `semodule -D`
    - B) `semodule -DB`
    - C) `setenforce 0`
    - D) `setsebool dontaudit off`

---

## Answer Key

### Chapter 22 (1-25)
1. B  2. B  3. C  4. B  5. C  
6. B  7. B  8. B  9. B  10. B  
11. C  12. C  13. B  14. B  15. B  
16. A  17. B  18. C  19. B  20. B  
21. B  22. C  23. B  24. B  25. B

### Chapter 23 (26-50)
26. B  27. C  28. B  29. A  30. C  
31. B  32. B  33. B  34. A  35. B  
36. B  37. B  38. B  39. B  40. A  
41. B  42. B  43. B  44. B  45. B  
46. B  47. A  48. B  49. B  50. B

### Chapter 24 (51-75)
51. B  52. B  53. B  54. B  55. B  
56. B  57. B  58. B  59. B  60. B  
61. B  62. B  63. B  64. B  65. B  
66. B  67. B  68. B  69. A  70. B  
71. A  72. B  73. B  74. B  75. B

---

## Scoring Guide

- **70-75 correct (93-100%):** Excellent! You have mastered Linux security techniques.
- **60-69 correct (80-92%):** Very Good! Strong understanding with minor gaps.
- **50-59 correct (67-79%):** Good! Solid foundation, review weak areas.
- **40-49 correct (53-66%):** Fair! Need more study on several topics.
- **Below 40 (< 53%):** Review the material and retake the quiz.

---

**Good luck!**
