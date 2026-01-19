# Init Systems Overview

## The Initialization Daemon

**Purpose:** First process started by kernel (PID 1)

**Responsibilities:**
*   Start services at boot
*   Manage running services
*   Spawn login processes (getty/mingetty)
*   Handle system shutdown

**Parent Process ID (PPID):** 0 (kernel)

**Process ID (PID):** 1

---

## Determining Your Init System

**Check PID 1:**
```bash
ps -e | head
```

**Output examples:**

**systemd:**
```
PID TTY     TIME CMD
  1 ?   00:04:36 systemd
```

**SysVinit:**
```
PID TTY     TIME CMD
  1 ?   00:00:01 init
```

---

## Init System Types

### Classic Init Systems

**1. SysVinit (System V Init)**
*   Traditional UNIX init system (1980s)
*   Based on UNIX System V
*   Uses runlevels (0-6)
*   Sequential service startup
*   Simple, easy to understand

**2. BSD Init**
*   Based on Berkeley UNIX
*   Similar to SysVinit
*   Originally used `/etc/ttytab`
*   Now uses `/etc/inittab` like SysVinit

### Modern Init Systems

**systemd**
*   Modern replacement (2010s)
*   Parallel service startup (faster boot)
*   Uses target units instead of runlevels
*   Manages more than just services
*   Backward compatible with SysVinit
*   Used by: RHEL 7+, Fedora 15+, Ubuntu 15.04+

**Note:** Upstart (used by older Ubuntu) is now deprecated

---

## Runlevels vs Target Units

### SysVinit Runlevels

| Runlevel | Name | Description |
|----------|------|-------------|
| **0** | Halt | Shut down system |
| **1 or S** | Single User | Root only, no network, CLI only |
| **2** | Multiuser | Users can log in, CLI, network varies |
| **3** | Extended Multiuser | Full multiuser, CLI, network active |
| **4** | User Defined | Customizable |
| **5** | Graphical | Full multiuser, GUI, network active |
| **6** | Reboot | Restart system |

### systemd Target Units

| Target Unit | Equivalent Runlevel | Description |
|-------------|---------------------|-------------|
| `poweroff.target` | 0 | Shut down |
| `rescue.target` | 1 | Single user mode |
| `multi-user.target` | 2, 3, 4 | Multiuser, CLI |
| `graphical.target` | 5 | Multiuser, GUI |
| `reboot.target` | 6 | Reboot |

---

## Backward Compatibility

**systemd maintains compatibility:**

**Runlevel target links:**
```bash
ls -l /lib/systemd/system/runlevel*.target
```

**Output:**
```
runlevel0.target -> poweroff.target
runlevel1.target -> rescue.target
runlevel2.target -> multi-user.target
runlevel3.target -> multi-user.target
runlevel4.target -> multi-user.target
runlevel5.target -> graphical.target
runlevel6.target -> reboot.target
```

**Old commands still work:**
*   `init 3` → `systemctl isolate multi-user.target`
*   `runlevel` → Shows current/previous runlevel
*   `telinit` → Same as `init`

---

## Key Differences

| Feature | SysVinit | systemd |
|---------|----------|---------|
| **Startup** | Sequential | Parallel |
| **Boot Speed** | Slower | Faster |
| **Organization** | Runlevels | Target units |
| **Config** | `/etc/inittab` | Unit files |
| **Commands** | `service`, `chkconfig` | `systemctl` |
| **Complexity** | Simple | Complex but powerful |
| **Manages** | Services only | Services, sockets, devices, mounts, etc. |

---

## Why systemd?

**Problems with classic init:**
*   Sequential startup (slow)
*   Static environment (trouble with hot-plug devices)
*   Limited dependency management
*   Inefficient with many services

**systemd advantages:**
*   Parallel startup (faster boot)
*   Dynamic environment handling
*   Advanced dependency management
*   Socket activation
*   Resource management (cgroups)
*   Unified logging (journald)

---

## Review Questions

1. What is the PID of the init daemon?

2. What are the seven SysVinit runlevels?

3. What systemd target is equivalent to runlevel 5?

4. True or False: systemd can only manage services.

5. How do you check which init system is running?

6. What runlevel should NEVER be set as default?

7. What command shows the current runlevel?

8. What is the main advantage of systemd over SysVinit?

---

## Quick Reference

```
Init Daemon:
├── PID: 1
├── PPID: 0 (kernel)
└── First process started

Check Init System:
└── ps -e | head

SysVinit Runlevels:
├── 0 → Halt
├── 1 → Single user
├── 2 → Multiuser (varies)
├── 3 → Extended multiuser (CLI)
├── 4 → User defined
├── 5 → Graphical
└── 6 → Reboot

systemd Targets:
├── poweroff.target    → Runlevel 0
├── rescue.target      → Runlevel 1
├── multi-user.target  → Runlevels 2,3,4
├── graphical.target   → Runlevel 5
└── reboot.target      → Runlevel 6

Key Differences:
├── SysVinit → Sequential, simple, runlevels
└── systemd  → Parallel, complex, targets

Backward Compatibility:
├── init 3           → Still works
├── runlevel         → Still works
└── Runlevel targets → Symbolic links
```
