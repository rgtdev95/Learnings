# SELinux Booleans and Policy Management

## What are SELinux Booleans

**Definition:**
*   Toggle switches for policy rules
*   Turn features on/off
*   No policy writing knowledge needed
*   No system reboot required

**Benefits:**
*   Easy policy modification
*   Quick changes
*   Safe adjustments
*   Reversible

---

## Viewing Booleans

**List all Booleans:**
```bash
getsebool -a
```

**Example output:**
```
abrt_anon_write --> off
abrt_handle_event --> off
httpd_can_connect_ftp --> off
httpd_can_network_connect --> off
xserver_object_manager --> off
```

**View specific Boolean:**
```bash
getsebool httpd_can_connect_ftp
```

**Output:**
```
httpd_can_connect_ftp --> off
```

---

## Setting Booleans

**Temporary change:**
```bash
setsebool boolean_name on
setsebool boolean_name off
```

**Permanent change:**
```bash
setsebool -P boolean_name on
```

**Boolean values:**
*   **On:** `on`, `1`, `true`
*   **Off:** `off`, `0`, `false`

**Example:**
```bash
# Temporary
setsebool httpd_can_network_connect on

# Permanent
setsebool -P httpd_can_network_connect on
```

---

## Common HTTP Booleans

**Network connectivity:**
```bash
httpd_can_network_connect --> off
```
*   Allow httpd to make network connections

**FTP connections:**
```bash
httpd_can_connect_ftp --> off
```
*   Allow httpd to connect to FTP servers

**Send mail:**
```bash
httpd_can_sendmail --> off
```
*   Allow httpd to send email

**User directories:**
```bash
httpd_enable_homedirs --> off
```
*   Allow httpd to access user home directories

**CGI scripts:**
```bash
httpd_enable_cgi --> on
```
*   Allow httpd to run CGI scripts

---

## Common FTP Booleans

**Anonymous write:**
```bash
ftpd_anon_write --> off
```
*   Allow anonymous FTP uploads

**Home directory access:**
```bash
ftp_home_dir --> off
```
*   Allow FTP access to user home directories

---

## Common Samba Booleans

**Home directories:**
```bash
samba_enable_home_dirs --> off
```
*   Allow Samba to share home directories

**Export all read/write:**
```bash
samba_export_all_rw --> off
```
*   Allow Samba to share all files read/write

---

## Common User Booleans

**Execute content:**
```bash
allow_user_exec_content --> on
```
*   Allow users to execute programs from home

**Example - Disable user execution:**
```bash
setsebool -P allow_user_exec_content off
```

---

## Policy Rule Packages (Modules)

**What are policy modules:**
*   Groups of policy rules
*   Also called packages
*   Installed with SELinux

**List modules:**
```bash
semodule -l
```

**Example output:**
```
abrt        1.4.1
accountsd   1.1.0
httpd       1.14.0
samba       1.12.0
ssh         1.10.0
```

---

## Policy Module Tools

**audit2allow:**
*   Generate policy rules from denials
*   Read audit logs
*   Create allow rules

**audit2why:**
*   Explain why access denied
*   Read audit logs
*   Generate descriptions

**checkmodule:**
*   Compile policy modules

**semodule:**
*   Manage policy modules
*   Load/unload modules

**semodule_package:**
*   Create policy module packages

---

## Creating Custom Policies

**Basic process:**

**1. Generate from denials:**
```bash
ausearch -c 'httpd' --raw | audit2allow -M my-httpd
```

**2. Review generated policy:**
```bash
cat my-httpd.te
```

**3. Install module:**
```bash
semodule -i my-httpd.pp
```

**Warning:**
*   Requires policy knowledge
*   Can compromise security
*   Test thoroughly
*   Use Booleans when possible

---

## Policy Documentation

**View policy docs:**

**Fedora/RHEL:**
```
file:///usr/share/doc/selinux-policy/html/index.html
```

**Ubuntu:**
```
file:///usr/share/doc/selinux-policy-doc/html/index.html
```

**Install docs:**
```bash
# Fedora/RHEL
yum install selinux-policy-doc

# Ubuntu
apt-get install selinux-policy-doc
```

---

## Managing Policy Modules

**List loaded modules:**
```bash
semodule -l
```

**Install module:**
```bash
semodule -i module.pp
```

**Remove module:**
```bash
semodule -r modulename
```

**Reload all modules:**
```bash
semodule -R
```

**Disable module:**
```bash
semodule -d modulename
```

**Enable module:**
```bash
semodule -e modulename
```

---

## GUI Configuration Tool

**Install:**
```bash
# Fedora/RHEL
yum install policycoreutils-gui

# Ubuntu
apt-get install policycoreutils
```

**Launch:**
```bash
system-config-selinux
```

**Features:**
*   Manage Booleans
*   View file contexts
*   Manage users
*   View policy modules
*   Network port labeling

---

## Review Questions

1. What is an SELinux Boolean?

2. What command lists all Booleans?

3. How do you make a Boolean change permanent?

4. What are the three ways to turn a Boolean on?

5. What command lists policy modules?

6. What tool generates policy rules from audit logs?

7. True or False: Changing Booleans requires a system reboot.

8. What's the GUI tool for managing SELinux?

---

## Quick Reference

```
Booleans:
├── Toggle switches for policies
├── On/off settings
├── No reboot required
└── Easy policy modification

View Booleans:
├── getsebool -a              → List all
└── getsebool boolean_name    → View specific

Set Booleans:
├── setsebool boolean on      → Temporary
└── setsebool -P boolean on   → Permanent

Boolean Values:
├── On:  on, 1, true
└── Off: off, 0, false

Common HTTP Booleans:
├── httpd_can_network_connect  → Network connections
├── httpd_can_connect_ftp      → FTP connections
├── httpd_can_sendmail         → Send email
├── httpd_enable_homedirs      → User home access
└── httpd_enable_cgi           → CGI scripts

Common FTP Booleans:
├── ftpd_anon_write  → Anonymous uploads
└── ftp_home_dir     → Home directory access

Common Samba Booleans:
├── samba_enable_home_dirs  → Share home dirs
└── samba_export_all_rw     → Share all read/write

User Booleans:
└── allow_user_exec_content → Execute from home

Policy Modules:
├── semodule -l      → List modules
├── semodule -i file → Install module
├── semodule -r name → Remove module
├── semodule -d name → Disable module
├── semodule -e name → Enable module
└── semodule -R      → Reload all

Policy Tools:
├── audit2allow   → Generate rules from denials
├── audit2why     → Explain denials
├── checkmodule   → Compile modules
└── semodule      → Manage modules

Create Custom Policy:
1. ausearch -c 'service' --raw | audit2allow -M my-policy
2. cat my-policy.te  (review)
3. semodule -i my-policy.pp  (install)

Policy Documentation:
├── Fedora/RHEL: file:///usr/share/doc/selinux-policy/html/
├── Ubuntu: file:///usr/share/doc/selinux-policy-doc/html/
└── Install: yum install selinux-policy-doc

GUI Tool:
├── Install: yum install policycoreutils-gui
└── Launch: system-config-selinux

Best Practices:
├── Use Booleans instead of custom policies
├── Make permanent with -P option
├── Test before making permanent
├── Document Boolean changes
└── Review policy docs before creating modules
```
