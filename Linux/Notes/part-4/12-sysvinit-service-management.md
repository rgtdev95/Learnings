# SysVinit Service Management

## /etc/inittab Configuration

**Location:** `/etc/inittab`

**Purpose:** Defines default runlevel and system initialization

**Example:**
```bash
# Default runlevel
id:5:initdefault:

# System initialization
si::sysinit:/etc/rc.d/rc.sysinit

# Runlevel scripts
l0:0:wait:/etc/rc.d/rc 0
l1:1:wait:/etc/rc.d/rc 1
l2:2:wait:/etc/rc.d/rc 2
l3:3:wait:/etc/rc.d/rc 3
l4:4:wait:/etc/rc.d/rc 4
l5:5:wait:/etc/rc.d/rc 5
l6:6:wait:/etc/rc.d/rc 6

# Ctrl-Alt-Delete
ca::ctrlaltdel:/sbin/shutdown -t3 -r now

# Getty processes
1:2345:respawn:/sbin/mingetty tty1
2:2345:respawn:/sbin/mingetty tty2
```

**⚠️ WARNING:** Never set default to 0 or 6 (causes halt/reboot loop)!

---

## Runlevel Directories

**Location:** `/etc/rc.d/rc#.d/` (where # = runlevel)

**Structure:**
```bash
/etc/rc.d/rc0.d/  # Halt
/etc/rc.d/rc1.d/  # Single user
/etc/rc.d/rc2.d/  # Multiuser
/etc/rc.d/rc3.d/  # Extended multiuser
/etc/rc.d/rc4.d/  # User defined
/etc/rc.d/rc5.d/  # Graphical
/etc/rc.d/rc6.d/  # Reboot
```

**File naming:**
*   **K##service** - Kill (stop) service
*   **S##service** - Start service
*   **##** - Order number (lower = earlier)

**Example:**
```bash
ls /etc/rc.d/rc5.d/
K15httpd   # Stop httpd (order 15)
S25cups    # Start cups (order 25)
S55sshd    # Start sshd (order 55)
```

---

## Service Scripts

**Location:** `/etc/rc.d/init.d/`

**Files in rc#.d are symbolic links:**
```bash
ls -l /etc/rc.d/rc5.d/S25cups
lrwxrwxrwx 1 root root 15 ... /etc/rc.d/rc5.d/S25cups -> ../init.d/cups
```

**Script structure:**
```bash
#!/bin/bash
# Service script

case "$1" in
  start)
    # Start service
    ;;
  stop)
    # Stop service
    ;;
  restart)
    # Restart service
    ;;
  status)
    # Show status
    ;;
esac
```

---

## chkconfig Command

**Purpose:** Manage service runlevels

**List all services:**
```bash
chkconfig --list
```

**Output:**
```
cups    0:off 1:off 2:on 3:on 4:on 5:on 6:off
sshd    0:off 1:off 2:on 3:on 4:on 5:on 6:off
```

**List specific service:**
```bash
chkconfig --list cups
```

**Enable service at runlevel:**
```bash
chkconfig --level 3 cups on
chkconfig --level 2345 cups on  # Multiple runlevels
```

**Disable service:**
```bash
chkconfig --level 5 cups off
```

**Add new service:**
```bash
chkconfig --add servicename
```

---

## service Command

**Purpose:** Control services immediately

**Check status:**
```bash
service cups status
```

**Start service:**
```bash
service cups start
```

**Stop service:**
```bash
service cups stop
```

**Restart service:**
```bash
service cups restart
```

**Reload configuration:**
```bash
service cups reload
```

**List all running services:**
```bash
service --status-all | grep running
```

---

## Changing Runlevels

**View current runlevel:**
```bash
runlevel
```

**Output:**
```
N 5  # N=none (fresh boot), 5=current
5 3  # 5=previous, 3=current
```

**Change runlevel immediately:**
```bash
init 3      # Switch to runlevel 3
telinit 3   # Same as init
```

**Set default runlevel:**

Edit `/etc/inittab`:
```bash
id:3:initdefault:  # Set default to runlevel 3
```

---

## Boot Process

1. **Kernel starts** → Launches init (PID 1)
2. **init reads** → `/etc/inittab`
3. **Runs** → `/etc/rc.d/rc.sysinit`
4. **Executes** → Scripts in `/etc/rc.d/rc#.d/`
   - K scripts (stop services)
   - S scripts (start services)
5. **Spawns** → Getty processes (login prompts)
6. **Starts** → X11 (if runlevel 5)

---

## Review Questions

1. What file defines the default runlevel?

2. What does K15httpd mean in /etc/rc.d/rc5.d/?

3. Where are actual service scripts stored?

4. What command enables a service at runlevel 3?

5. True or False: service restart stops then starts a service.

6. What does runlevel N mean?

7. How do you change to runlevel 3 immediately?

8. What's the difference between service and chkconfig?

---

## Quick Reference

```
Configuration:
└── /etc/inittab → Default runlevel, init settings

Service Scripts:
└── /etc/rc.d/init.d/ → Actual scripts

Runlevel Directories:
└── /etc/rc.d/rc#.d/ → Symbolic links (K/S scripts)

chkconfig (Persistent):
├── chkconfig --list           → List all
├── chkconfig --list service   → List specific
├── chkconfig --level 3 svc on → Enable
├── chkconfig --level 5 svc off → Disable
└── chkconfig --add service    → Add new

service (Immediate):
├── service svc status   → Check status
├── service svc start    → Start
├── service svc stop     → Stop
├── service svc restart  → Restart
└── service svc reload   → Reload config

Runlevel Commands:
├── runlevel             → Show current/previous
├── init #               → Change runlevel
└── telinit #            → Same as init

Default Runlevel:
└── Edit /etc/inittab: id:3:initdefault:
```
