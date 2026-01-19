# PAM (Pluggable Authentication Modules)

## What is PAM

**Definition:**
*   Pluggable Authentication Modules
*   Invented by Sun Microsystems (Solaris)
*   Linux-PAM project began 1997
*   Centralized authentication management

**Purpose:**
*   Simplifies authentication
*   Centralized configuration
*   Flexible authentication policies

**Benefits:**
*   No need to recompile applications
*   Changes made in configuration files
*   Simplified administration
*   Flexible authentication criteria

---

## PAM-Aware Applications

**Check if PAM-aware:**
```bash
ldd /usr/bin/crontab | grep pam
```

**Output if PAM-aware:**
```
libpam.so.0 => /lib64/libpam.so.0 (0x00007fbee19ce000)
```

**PAM library:**
*   `libpam.so`
*   Linked to PAM-aware applications

---

## PAM Authentication Process

**Steps:**

**1. Subject requests access:**
*   User or process
*   Requests application access

**2. Configuration file opened:**
*   Application's PAM config file
*   Contains access policy
*   Located in `/etc/pam.d/`

**3. PAM modules invoked:**
*   Modules listed in stack
*   Called in order listed

**4. Modules return status:**
*   Success or failure
*   Each module returns status

**5. Stack continues:**
*   Not stopped by single failure
*   Depends on control flags

**6. Combined result:**
*   All statuses combined
*   Final success or failure

---

## PAM Configuration Files

**Location:** `/etc/pam.d/`

**File types:**

**Application files:**
*   One per PAM-aware application
*   Example: `crond`, `sshd`, `sudo`

**System event files:**
*   System-wide authentication
*   Example: `system-auth`, `password-auth`

**Format:**
```
context  control_flag  PAM_module  [module_options]
```

---

## PAM Contexts (Module Interfaces)

**Four types:**

| Context | Service |
|---------|---------|
| `auth` | Authentication management (verify passwords) |
| `account` | Account validation (time restrictions, expiration) |
| `password` | Password management (length, complexity) |
| `session` | Session setup/teardown (logging, limits) |

---

## PAM Control Flags

**Simple keywords:**

| Flag | Behavior |
|------|----------|
| `required` | Failure returned after stack completes |
| `requisite` | Failure returned immediately |
| `sufficient` | Success returned immediately |
| `optional` | Used as tiebreaker |
| `include` | Include another config file's stack |
| `substack` | Include as substack |

**required:**
*   Failure logged
*   Stack continues
*   Failure returned at end

**requisite:**
*   Failure stops stack immediately
*   Be careful with placement

**sufficient:**
*   Success stops stack immediately
*   Failure ignored
*   Be careful with placement

**optional:**
*   Only matters for tiebreaker
*   Ignored if clear success/failure

**include:**
*   Pull in entire stack from another file
*   Treat as part of this stack

**substack:**
*   Like include, but isolated
*   Errors don't affect main stack

---

## PAM Modules

**Location:** `/usr/lib64/security/` (64-bit)

**List modules:**
```bash
ls /usr/lib64/security/pam*.so
```

**Ubuntu:**
```bash
find / -name "pam*.so"
```

**Common modules:**
*   `pam_unix.so` - Traditional Unix authentication
*   `pam_cracklib.so` - Password strength checking
*   `pam_limits.so` - Resource limits
*   `pam_time.so` - Time restrictions
*   `pam_access.so` - Access control
*   `pam_deny.so` - Deny access
*   `pam_permit.so` - Permit access

**Module documentation:**
```bash
man pam_modulename
```

---

## The "other" Configuration File

**Purpose:**
*   Default for apps without config file
*   Implements Implicit Deny
*   Security safeguard

**Check existence:**
```bash
ls /etc/pam.d/other
```

**Content:**
```
#%PAM-1.0
auth      required  pam_deny.so
account   required  pam_deny.so
password  required  pam_deny.so
session   required  pam_deny.so
```

**Implicit Deny:**
*   If no config file exists
*   All access denied
*   Security best practice

---

## Resource Limits with PAM

**Configuration:** `/etc/security/limits.conf`

**Format:**
```
<domain>  <type>  <item>  <value>
```

**Domain:**
*   Username or group
*   `*` = all users
*   `@group` = group name

**Type:**
*   `hard` - Cannot be exceeded
*   `soft` - Can be exceeded temporarily

**Common items:**
*   `nproc` - Max processes
*   `maxlogins` - Max simultaneous logins
*   `core` - Core file size
*   `fsize` - Max file size
*   `cpu` - CPU time (minutes)
*   `data` - Max data size

**Examples:**
```
# Prevent fork bombs
@faculty  hard  nproc  50

# Limit logins
johndoe   hard  maxlogins  1

# No core dumps
*         soft  core  0
```

**Override directory:** `/etc/security/limits.d/*.conf`

**PAM module:** `pam_limits.so`

**Verify module used:**
```bash
grep "pam_limits" /etc/pam.d/*
```

---

## Time Restrictions with PAM

**Configuration:** `/etc/security/time.conf`

**Format:**
```
services;ttys;users;times
```

**Example:**
```
# Login only weekdays 7am-7pm
login;*;!root;Wd0700-1900
```

**Fields:**
*   `services` - Service name (login, sshd, etc.)
*   `ttys` - Terminals (* = all)
*   `users` - Usernames (!root = except root)
*   `times` - Time restrictions

**Time format:**
*   `Wd` - Weekdays (Mon-Fri)
*   `Wk` - Weekend (Sat-Sun)
*   `Al` - All days
*   `0700-1900` - 7am to 7pm

**PAM module:** `pam_time.so`

**Add to system-auth:**
```
account  required  pam_time.so
```

---

## Password Strength with PAM

**Module:** `pam_cracklib.so`

**Installation (Ubuntu):**
```bash
apt-get install libpam-cracklib
```

**Configuration:** `/etc/pam.d/system-auth`

**Example:**
```
password  requisite  pam_cracklib.so minlen=10 dcredit=-2
```

**Common options:**

| Option | Description |
|--------|-------------|
| `minlen=N` | Minimum length |
| `dcredit=N` | Digit credit/requirement |
| `ucredit=N` | Uppercase credit/requirement |
| `lcredit=N` | Lowercase credit/requirement |
| `ocredit=N` | Other char credit/requirement |
| `difok=N` | Different from old password |
| `retry=N` | Retry attempts |
| `reject_username` | Reject if contains username |
| `enforce_for_root` | Apply to root user |

**Credit values:**
*   `N >= 0` - Maximum credit
*   `N < 0` - Minimum requirement

**Example:**
```
password requisite pam_cracklib.so \
  minlen=12 \
  dcredit=-2 \
  ucredit=-1 \
  lcredit=-1 \
  ocredit=-1
```

---

## Restricting su with PAM

**Goal:**
*   Disable `su` command
*   Force `sudo` usage
*   Enable tracking

**Steps:**

**1. Don't assign users to wheel group**

**2. Edit `/etc/pam.d/su`:**

Uncomment:
```
auth  required  pam_wheel.so use_uid
```

**Result:**
*   Only wheel group can use `su`
*   No one in wheel group = no one can use `su`
*   Must use `sudo` instead

---

## Managing PAM System Files

**Warning:**
*   Some files managed by `authselect`
*   Local changes may be overwritten

**Check which files:**
```bash
grep "authselect" /etc/pam.d/*
```

**Affected files:**
*   `fingerprint-auth`
*   `password-auth`
*   `postlogin`
*   `smartcard-auth`
*   `system-auth`

**Symbolic links:**
```bash
ls -l /etc/pam.d/system-auth
# Output: lrwxrwxrwx ... system-auth -> /etc/authselect/system-auth
```

**Ubuntu alternative:**
*   `pam-auth-config` utility
*   Can overwrite with `--force`

---

## Review Questions

1. What does PAM stand for?

2. How do you check if an application is PAM-aware?

3. What are the four PAM contexts?

4. What's the difference between required and requisite control flags?

5. Where are PAM configuration files located?

6. What file implements Implicit Deny?

7. What PAM module enforces resource limits?

8. True or False: PAM modules are located in /etc/pam.d/.

---

## Quick Reference

```
PAM Basics:
├── Pluggable Authentication Modules
├── Centralized authentication
├── Configuration in /etc/pam.d/
└── Modules in /usr/lib64/security/

Check PAM-Aware:
└── ldd /path/to/command | grep pam

Configuration Format:
└── context  control_flag  PAM_module  [options]

Contexts:
├── auth     → Authentication
├── account  → Account validation
├── password → Password management
└── session  → Session setup

Control Flags:
├── required   → Fail after stack completes
├── requisite  → Fail immediately
├── sufficient → Success immediately
├── optional   → Tiebreaker
├── include    → Include another file
└── substack   → Include as substack

Common Modules:
├── pam_unix.so      → Unix authentication
├── pam_cracklib.so  → Password strength
├── pam_limits.so    → Resource limits
├── pam_time.so      → Time restrictions
├── pam_deny.so      → Deny access
└── pam_permit.so    → Permit access

Resource Limits (/etc/security/limits.conf):
├── Format: domain type item value
├── Example: @faculty hard nproc 50
└── Module: pam_limits.so

Time Restrictions (/etc/security/time.conf):
├── Format: services;ttys;users;times
├── Example: login;*;!root;Wd0700-1900
└── Module: pam_time.so

Password Strength:
├── Module: pam_cracklib.so
├── minlen=N   → Minimum length
├── dcredit=N  → Digit requirement
├── ucredit=N  → Uppercase requirement
└── Example: password requisite pam_cracklib.so minlen=12 dcredit=-2

Restrict su:
1. Don't assign users to wheel group
2. Edit /etc/pam.d/su
3. Uncomment: auth required pam_wheel.so use_uid

Module Documentation:
└── man pam_modulename

List Modules:
└── ls /usr/lib64/security/pam*.so

Implicit Deny:
├── File: /etc/pam.d/other
└── Denies access if no config exists

authselect Warning:
├── Check: grep "authselect" /etc/pam.d/*
├── Affected files may be overwritten
└── Use authselect to make changes

Best Practices:
├── Test changes in non-production
├── Keep "other" file with Implicit Deny
├── Document PAM modifications
├── Regular security audits
└── Use resource limits to prevent fork bombs
```
