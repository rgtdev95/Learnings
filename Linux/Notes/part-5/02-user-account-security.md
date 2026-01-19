# User Account Security

## One User Per User Account

**Principle:** Enforce accountability

**Why it matters:**
*   Multiple people sharing account = no accountability
*   Cannot prove who did what
*   Repudiation problem
*   Security audit trail broken

**Best practice:**
*   Each person gets unique account
*   No shared accounts
*   Track individual actions
*   Enable proper auditing

---

## Limiting Root Access

**Problem with shared root:**
*   Multiple people with root password
*   Cannot track individual use
*   Repudiation situation
*   No accountability

**Solution: Use sudo**

**Benefits of sudo:**
*   Root password stays secret
*   Fine-tune command access
*   All use logged in `/var/log/secure`
*   Failed attempts logged
*   systemd journal tracks all access

**View sudo attempts:**
```bash
# Watch live
journalctl -f

# View secure log
tail -f /var/log/secure
```

**Configure sudo:**
```bash
# Edit sudoers file (ALWAYS use visudo)
visudo
```

**Example sudoers entries:**
```
# Allow user to run all commands
john ALL=(ALL) ALL

# Allow group to run specific commands
%admins ALL=/usr/sbin/useradd, /usr/sbin/userdel

# No password required
jane ALL=(ALL) NOPASSWD: ALL

# Specific commands only
bob ALL=/bin/systemctl restart httpd
```

**Warning:**
*   Even limited sudo can lead to full root access
*   Determined users can find workarounds
*   Monitor sudo usage carefully

---

## Remote Log Server

**Enhanced accountability:**

**Setup:**
*   Send `/var/log/secure` to remote server
*   Administrators have no access to remote server
*   Prevents covering tracks

**Benefits:**
*   Misuse of root privilege logged
*   Attached to particular user
*   Cannot be deleted by local admin
*   Audit trail preserved

---

## Setting Account Expiration Dates

**When to use:**
*   Consultants
*   Interns
*   Temporary employees
*   Contract workers

**Why it matters:**
*   Safeguard if you forget to remove account
*   Automatic access termination
*   Reduces security risk

**Set expiration with usermod:**
```bash
# Format: yyyy-mm-dd
usermod -e 2021-01-01 tim
```

**Verify expiration:**
```bash
# Check account expiration
chage -l tim | grep Account

# Output
Account expires                        : Jan 01, 2021
```

**View all expiration info:**
```bash
chage -l username
```

---

## Removing Unused Accounts

**Proper removal process:**

**1. Find files owned by account:**
```bash
find / -user username
```

**2. Expire or disable account:**
```bash
# Expire immediately
usermod -e 1 username

# Lock account
usermod -L username
passwd -l username
```

**3. Back up files:**
```bash
tar -czf username-backup.tar.gz /home/username
rsync -av /home/username /backups/
```

**4. Remove or reassign files:**
```bash
# Reassign to new owner
find / -user olduser -exec chown newuser {} \;

# Or remove
rm -rf /home/username
```

**5. Delete account:**
```bash
# Remove account and home directory
userdel -r username

# Remove account only
userdel username
```

---

## Finding Expired Accounts

**Problem:**
*   Forgotten expired accounts
*   Potential backdoors
*   Malicious user could renew account

**Account expiration in /etc/shadow:**
*   8th field in each record
*   Days since January 1, 1970
*   Not human-readable

**Find expired accounts:**

**Step 1: Set TODAY variable**
```bash
TODAY=$(echo $(($(date --utc --date "$1" +%s)/86400)))
echo $TODAY
# Output: 16373
```

**Step 2: View expiration dates**
```bash
# See all accounts and expiration
gawk -F: '{print $1,$8}' /etc/shadow
```

**Example output:**
```
chrony
tcpdump
johndoe
Consultant 13819
Intern 13911
```

**Step 3: Find expired accounts**
```bash
# Show only expired accounts
gawk -F: '{if (($8 > 0) && ($TODAY > $8)) print $1}' /etc/shadow
```

**Example output:**
```
Consultant
Intern
```

**Remove expired accounts:**
```bash
userdel -r Consultant
userdel -r Intern
```

---

## Account Security Best Practices

**Account creation:**
*   One user per account
*   Strong naming convention
*   Document account purpose

**Access control:**
*   Limit root access
*   Use sudo for privilege escalation
*   Log all privileged access
*   Remote logging for accountability

**Temporary accounts:**
*   Always set expiration dates
*   Review regularly
*   Remove promptly when no longer needed

**Regular audits:**
*   Find expired accounts monthly
*   Check for unused accounts
*   Verify account ownership
*   Review sudo permissions

**Documentation:**
*   Maintain account inventory
*   Document account purposes
*   Track expiration dates
*   Record removal dates

---

## Review Questions

1. Why should each person have their own user account?

2. What are three benefits of using sudo instead of sharing root password?

3. What command sets an account expiration date?

4. How do you verify an account's expiration date?

5. What are the 5 steps to properly remove a user account?

6. Where is account expiration information stored?

7. True or False: The /etc/shadow file stores expiration dates in human-readable format.

8. What command finds all files owned by a specific user?

---

## Quick Reference

```
Account Principles:
├── One user per account
├── Limit root access
├── Use sudo
└── Set expiration for temporary accounts

sudo Configuration:
├── Edit: visudo
├── File: /etc/sudoers
├── Log: /var/log/secure
└── Journal: journalctl -f

sudo Examples:
├── user ALL=(ALL) ALL              → Full access
├── user ALL=/usr/bin/systemctl     → Specific command
├── user ALL=(ALL) NOPASSWD: ALL    → No password
└── %group ALL=/usr/sbin/useradd    → Group access

Account Expiration:
├── Set: usermod -e yyyy-mm-dd username
├── Check: chage -l username | grep Account
└── Format: yyyy-mm-dd

Find Expired Accounts:
1. TODAY=$(echo $(($(date --utc +%s)/86400)))
2. gawk -F: '{print $1,$8}' /etc/shadow
3. gawk -F: '{if (($8 > 0) && ($TODAY > $8)) print $1}' /etc/shadow

Account Removal Process:
1. find / -user username           → Find files
2. usermod -L username             → Lock account
3. tar -czf backup.tar.gz /home/user → Backup
4. chown newuser files             → Reassign files
5. userdel -r username             → Delete account

Disable Account:
├── usermod -L username  → Lock
├── passwd -l username   → Lock password
└── usermod -e 1 username → Expire immediately

View sudo Usage:
├── tail -f /var/log/secure
├── journalctl -f
└── grep sudo /var/log/secure

Best Practices:
├── One user per account
├── Use sudo, not root login
├── Set expiration for temp accounts
├── Regular account audits
├── Remove unused accounts
└── Remote logging for accountability
```
