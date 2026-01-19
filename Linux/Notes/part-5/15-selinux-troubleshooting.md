# SELinux Troubleshooting

## SELinux Logging

### Access Vector Cache (AVC)

**What is AVC:**
*   Cache for policy rule reviews
*   Stores access decisions
*   Improves performance

**AVC Denial:**
*   Access denied by SELinux
*   Logged to file
*   Contains denial details

---

## Log File Locations

**Depends on running daemons:**

**If auditd running:**
*   Location: `/var/log/audit/audit.log`
*   Detailed AVC messages
*   Raw format

**If rsyslogd running (no auditd):**
*   Location: `/var/log/messages`
*   Less detailed
*   Syslog format

**If both running + setroubleshootd:**
*   Both log files used
*   `/var/log/messages` has user-friendly format
*   `/var/log/audit/audit.log` has raw format

---

## Reviewing Audit Logs

### Using aureport

**Check for AVC denials:**
```bash
aureport | grep AVC
```

**Example output:**
```
Number of AVC's: 1
```

### Using ausearch

**Search for AVC denials:**
```bash
ausearch -m avc
```

**Example output:**
```
type=AVC msg=audit(1580397837.344:274): avc: denied { getattr } for
  pid=1067 comm="httpd" path="/var/myserver/services" dev="dm-0" ino=655836
  scontext=system_u:system_r:httpd_t:s0
  tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
```

**Key fields:**
*   `type=AVC` - AVC denial message
*   `avc: denied` - Access denied
*   `comm="httpd"` - Command/process
*   `path="/var/myserver/services"` - File/resource
*   `scontext` - Source context (who)
*   `tcontext` - Target context (what)

---

## Reviewing Messages Log

**Using journalctl:**
```bash
journalctl | grep AVC
```

**Using grep:**
```bash
grep AVC /var/log/messages
```

---

## Using sealert

**Analyze audit log:**
```bash
sealert -a /var/log/audit/audit.log
```

**Example output:**
```
SELinux is preventing httpd from getattr access on the file
/var/myserver/services.

Plugin catchall (100. confidence) suggests

If you believe that httpd should be allowed getattr access on the
services file by default.
Then you should report this as a bug.
You can generate a local policy module to allow this access.
Do allow this access for now by executing:
# ausearch -c 'httpd' --raw | audit2allow -M my-httpd
# semodule -X 300 -i my-httpd.pp
```

**Benefits:**
*   User-friendly explanations
*   Suggested solutions
*   Commands to fix

---

## Common SELinux Problems

### 1. Check DAC First

**Always verify traditional permissions:**
```bash
ls -l filename
```

**Check:**
*   Owner correct?
*   Group correct?
*   Permissions correct (rwx)?

**Remember:**
*   DAC checked before SELinux
*   If DAC denies, SELinux not checked

---

### 2. Nonstandard Directory

**Problem:**
*   Service files in nonstandard location
*   SELinux denies access
*   Example: HTML files in `/abc/www/html` instead of `/var/www/html`

**Solution:**

**Add file context:**
```bash
semanage fcontext -a -t httpd_sys_content_t "/abc/www/html(/.*)?"
```

**Apply context:**
```bash
restorecon -R -v /abc/www/html
```

**Verify:**
```bash
ls -Z /abc/www/html
```

---

### 3. Nonstandard Port

**Problem:**
*   Service on nonstandard port
*   Service fails to start
*   Example: SSH on port 47347 instead of 22

**Solution:**

**Find context type:**
```bash
semanage port -l | grep ssh
```

**Output:**
```
ssh_port_t    tcp    22
```

**Add new port:**
```bash
semanage port -a -t ssh_port_t -p tcp 47347
```

**Verify:**
```bash
semanage port -l | grep ssh
```

**Output:**
```
ssh_port_t    tcp    47347, 22
```

**Update service config:**
```bash
# Edit /etc/ssh/sshd_config
Port 47347

# Restart service
systemctl restart sshd
```

---

### 4. Moved Files Lost Context

**Problem:**
*   Moved file with `mv` command
*   File keeps old context
*   Wrong context for new location

**Example:**
```bash
# Copy to /tmp
cp /etc/test.txt /tmp/

# Move back
mv /tmp/test.txt /etc/

# Wrong context now
ls -Z /etc/test.txt
# Shows: user_tmp_t (wrong!)
```

**Solution:**
```bash
restorecon /etc/test.txt
```

**Verify:**
```bash
ls -Z /etc/test.txt
# Shows: etc_t (correct!)
```

---

### 5. Incorrect Booleans

**Problem:**
*   Feature not working
*   Boolean set incorrectly
*   Example: httpd can't connect to network

**Diagnose:**
```bash
getsebool -a | grep httpd
```

**Output:**
```
httpd_can_network_connect --> off
```

**Solution:**
```bash
setsebool -P httpd_can_network_connect on
```

**Verify:**
```bash
getsebool httpd_can_network_connect
```

---

## Troubleshooting Logging Issues

**Check daemons running:**
```bash
systemctl status auditd
systemctl status rsyslog
systemctl status setroubleshoot
```

**Start if not running:**
```bash
systemctl start auditd
systemctl enable auditd
```

**Disable dontaudit rules (temporarily):**
```bash
semodule -DB
```

**Purpose:**
*   dontaudit rules hide some denials
*   Can cause troubleshooting issues
*   Temporarily disable to see all denials

**Re-enable:**
```bash
semodule -B
```

---

## Troubleshooting Workflow

**1. Check if SELinux is the problem:**
```bash
# Set to permissive temporarily
setenforce 0

# Test if issue resolved
# If yes, SELinux is the problem
# If no, not SELinux

# Set back to enforcing
setenforce 1
```

**2. Check logs:**
```bash
ausearch -m avc
journalctl | grep AVC
sealert -a /var/log/audit/audit.log
```

**3. Identify problem:**
*   Wrong file context?
*   Wrong port context?
*   Boolean off?
*   Custom policy needed?

**4. Apply fix:**
*   Fix file context
*   Fix port context
*   Enable Boolean
*   Create policy module

**5. Test:**
*   Verify fix works
*   Check no new denials

---

## Best Practices

**Start with permissive:**
*   Run system in permissive mode
*   Review logs
*   Fix issues
*   Switch to enforcing

**One change at a time:**
*   Make single change
*   Test thoroughly
*   Document change
*   Move to next issue

**Test environment:**
*   Test changes in non-production
*   Verify no side effects
*   Then apply to production

**Document everything:**
*   Record all changes
*   Note reasons
*   Keep change log

---

## Getting Help

**Man pages:**
```bash
man -k selinux
man selinux
man httpd_selinux  # Service-specific
```

**Service-specific man pages:**
*   `man httpd_selinux`
*   `man ftpd_selinux`
*   `man samba_selinux`

**Online resources:**
*   Red Hat docs: http://docs.redhat.com
*   Fedora docs: http://docs.fedoraproject.org
*   SELinux Project: http://selinuxproject.org

---

## Review Questions

1. What is an AVC denial?

2. Where are AVC denials logged if auditd is running?

3. What command searches for AVC denials in audit log?

4. What should you always check before troubleshooting SELinux?

5. How do you fix a file in a nonstandard directory?

6. How do you allow a service on a nonstandard port?

7. True or False: Moving files with `mv` preserves SELinux contexts.

8. What command temporarily disables dontaudit rules?

---

## Quick Reference

```
AVC Denial:
├── Access denied by SELinux
├── Logged to file
└── Contains denial details

Log Locations:
├── auditd running: /var/log/audit/audit.log
├── rsyslogd only: /var/log/messages
└── Both + setroubleshoot: both files

Check for Denials:
├── aureport | grep AVC
├── ausearch -m avc
├── journalctl | grep AVC
└── sealert -a /var/log/audit/audit.log

AVC Message Key Fields:
├── type=AVC → AVC denial
├── avc: denied → Access denied
├── comm="httpd" → Process
├── path="/file" → Resource
├── scontext → Source context
└── tcontext → Target context

Common Problems:
1. DAC permissions wrong
2. Nonstandard directory
3. Nonstandard port
4. Moved files lost context
5. Incorrect Booleans

Nonstandard Directory Fix:
1. semanage fcontext -a -t type "path(/.*)?"
2. restorecon -R -v path
3. ls -Z path  (verify)

Nonstandard Port Fix:
1. semanage port -l | grep service
2. semanage port -a -t type -p tcp port
3. Update service config
4. Restart service

Restore File Context:
└── restorecon filename

Fix Boolean:
└── setsebool -P boolean_name on

Troubleshooting Workflow:
1. setenforce 0  (test if SELinux issue)
2. Check logs
3. Identify problem
4. Apply fix
5. Test
6. setenforce 1

Check Daemons:
├── systemctl status auditd
├── systemctl status rsyslog
└── systemctl status setroubleshoot

Disable dontaudit (temporary):
├── semodule -DB  → Disable
└── semodule -B   → Re-enable

Get Help:
├── man -k selinux
├── man service_selinux
├── Red Hat docs: docs.redhat.com
├── Fedora docs: docs.fedoraproject.org
└── SELinux Project: selinuxproject.org

Best Practices:
├── Start with permissive mode
├── One change at a time
├── Test in non-production
├── Document all changes
└── Review logs regularly
```
