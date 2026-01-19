# SELinux Basics

## What is SELinux

**Security Enhanced Linux:**
*   Developed by NSA
*   Released to open source (2000)
*   Security enhancement module
*   Default in RHEL/Fedora

**Purpose:**
*   Additional security layer
*   Role Based Access Control (RBAC)
*   Process sandboxing
*   Mandatory Access Control (MAC)

---

## DAC vs MAC

### Discretionary Access Control (DAC)

**Traditional Linux security:**
*   Process can access any open resource
*   Based on file permissions (rwx)
*   Owner controls access
*   Less restrictive

### Mandatory Access Control (MAC)

**SELinux security:**
*   Process only accesses explicitly allowed resources
*   Based on assigned roles and policies
*   System-wide policy controls access
*   More restrictive

**Relationship:**
*   SELinux doesn't replace DAC
*   DAC checked first
*   If DAC denies, SELinux not checked
*   If DAC allows, SELinux checked

---

## SELinux Benefits

**Role Based Access Control (RBAC):**
*   Strongest access control model
*   Access based on role, not username

**Least Privilege:**
*   Subjects get minimal needed privileges
*   Limits accidental/intentional damage
*   Reduces attack surface

**Process Sandboxing:**
*   Each process runs in own domain
*   Cannot access other processes
*   Isolated execution environment

**Test Mode:**
*   Permissive mode available
*   Logs violations without blocking
*   Test before enforcement

---

## SELinux vs AppArmor

**SELinux:**
*   Default: RHEL, Fedora, CentOS
*   More complex
*   More granular control

**AppArmor:**
*   Default: Ubuntu, SUSE
*   Simpler
*   Path-based security

**Ubuntu SELinux:**
```bash
# Install (not recommended by Ubuntu)
apt-get install selinux
```

**Note:** Ubuntu Wiki suggests not using SELinux package

---

## How SELinux Works

**Analogy: Guard at door**

**Process:**
1. Subject wants to access object
2. Subject presents ID badge (security context)
3. Guard (SELinux) reviews access rules (policy)
4. Access allowed or denied

**Components:**
*   Security contexts (ID badges)
*   Policy rules (access rules manual)
*   Enforcement engine (guard)

---

## SELinux Operational Modes

### Disabled Mode

**Characteristics:**
*   SELinux turned off
*   Only DAC used
*   No enhanced security

**When to use:**
*   Enhanced security not required
*   Troubleshooting
*   Legacy applications

**Warning:**
*   Re-enabling requires filesystem relabel
*   Long reboot process

**Disable:**
```bash
# Edit /etc/selinux/config
SELINUX=disabled
# Reboot required
```

### Permissive Mode

**Characteristics:**
*   SELinux turned on
*   Policy rules NOT enforced
*   Violations logged, not blocked

**When to use:**
*   Auditing current policies
*   Testing new applications
*   Testing new policy rules
*   Troubleshooting

**Set permissive:**
```bash
# Temporary
setenforce 0

# Permanent (edit /etc/selinux/config)
SELINUX=permissive
```

### Enforcing Mode

**Characteristics:**
*   SELinux turned on
*   All policy rules enforced
*   Violations blocked and logged

**Set enforcing:**
```bash
# Temporary
setenforce 1

# Permanent (edit /etc/selinux/config)
SELINUX=enforcing
```

---

## Checking SELinux Status

**Get current mode:**
```bash
getenforce
# Output: Enforcing
```

**Get detailed status:**
```bash
sestatus
```

**Example output:**
```
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      31
```

---

## SELinux Policy Types

### Targeted Policy (Default)

**Purpose:**
*   Restrict targeted daemons
*   Sandbox network services
*   Less restrictive for users

**Characteristics:**
*   Default policy
*   Focuses on network services
*   Unconfined processes run in `unconfined_t`
*   Only DAC for unconfined processes

**Services targeted:**
*   Web servers (httpd)
*   File servers (Samba, NFS)
*   FTP servers
*   SSH servers

### MLS Policy (Multi-Level Security)

**Purpose:**
*   Enforce Bell-LaPadula model
*   Security clearances
*   Classification levels

**Characteristics:**
*   More restrictive
*   Security clearance required
*   Classification levels on objects
*   Government/military use

**Bell-LaPadula Model:**
*   Information confidentiality
*   Clearance-based access
*   Top Secret, Secret, Confidential

### Minimum Policy

**Purpose:**
*   Minimal resource usage
*   Low-memory devices
*   Testing single daemon

**Characteristics:**
*   Base policy only
*   Similar to Targeted
*   Fewer resources
*   Smartphones, embedded devices

---

## Setting Policy Type

**Configuration file:** `/etc/selinux/config`

**Options:**
```bash
SELINUXTYPE=targeted  # Default
SELINUXTYPE=mls       # Multi-Level Security
SELINUXTYPE=minimum   # Minimal policy
```

**Check if installed:**
```bash
yum list selinux-policy-mls
yum list selinux-policy-minimum
```

---

## Configuration File

**Location:** `/etc/selinux/config`

**Example:**
```bash
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - SELinux is fully disabled.
SELINUX=enforcing

# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

---

## Review Questions

1. What does SELinux stand for?

2. What's the difference between DAC and MAC?

3. What are the three SELinux operational modes?

4. What command shows the current SELinux mode?

5. What's the default SELinux policy type?

6. What does permissive mode do?

7. True or False: SELinux replaces traditional Linux security.

8. What happens when you switch from disabled to enforcing mode?

---

## Quick Reference

```
SELinux:
├── Security Enhanced Linux
├── Developed by NSA
├── Default: RHEL, Fedora
└── Additional security layer

DAC vs MAC:
├── DAC (Discretionary Access Control)
│   ├── Traditional Linux
│   ├── File permissions (rwx)
│   └── Checked first
└── MAC (Mandatory Access Control)
    ├── SELinux
    ├── Role-based
    └── Checked after DAC allows

Operational Modes:
├── disabled   → SELinux off
├── permissive → Log violations, don't block
└── enforcing  → Enforce all policies

Check Status:
├── getenforce  → Current mode
└── sestatus    → Detailed status

Set Mode (Temporary):
├── setenforce 0  → Permissive
└── setenforce 1  → Enforcing

Set Mode (Permanent):
└── Edit /etc/selinux/config
    ├── SELINUX=disabled
    ├── SELINUX=permissive
    └── SELINUX=enforcing

Policy Types:
├── targeted → Default, network services
├── mls      → Multi-Level Security
└── minimum  → Minimal, low-memory

Set Policy Type:
└── Edit /etc/selinux/config
    ├── SELINUXTYPE=targeted
    ├── SELINUXTYPE=mls
    └── SELINUXTYPE=minimum

Configuration:
└── /etc/selinux/config

Benefits:
├── RBAC (Role Based Access Control)
├── Least privilege
├── Process sandboxing
└── Test mode (permissive)

Filesystem Relabel:
├── Occurs when switching from disabled
├── Checks/fixes security contexts
├── Can take long time
└── Automatic after reboot

Best Practices:
├── Start with permissive mode
├── Review logs before enforcing
├── Test in non-production first
└── One change at a time
```
