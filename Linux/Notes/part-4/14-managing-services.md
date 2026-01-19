# Managing Services

## Checking Service Status

### SysVinit

**Check all services:**
```bash
chkconfig --list
```

**Check specific service:**
```bash
chkconfig --list cups
service cups status
```

**List running services:**
```bash
service --status-all | grep running | sort
```

### systemd

**List all services:**
```bash
systemctl list-unit-files --type=service
systemctl list-unit-files --type=service | grep -v disabled
```

**Check specific service:**
```bash
systemctl status cups.service
```

**List active services:**
```bash
systemctl list-units --type=service --state=active
```

---

## Starting and Stopping Services

### SysVinit

**Start:**
```bash
service cups start
```

**Stop:**
```bash
service cups stop
```

**Restart:**
```bash
service cups restart
```

**Reload:**
```bash
service cups reload
```

**Note:** Reload doesn't stop service, just reloads config

### systemd

**Start:**
```bash
systemctl start cups.service
```

**Stop:**
```bash
systemctl stop cups.service
```

**Restart:**
```bash
systemctl restart cups.service
```

**Conditional restart (only if running):**
```bash
systemctl condrestart cups.service
# or
systemctl try-restart cups.service
```

**Reload:**
```bash
systemctl reload cups.service
```

**Reload or restart (if reload not supported):**
```bash
systemctl reload-or-restart cups.service
```

---

## Enabling and Disabling Services

### SysVinit

**Enable at specific runlevel:**
```bash
chkconfig --level 3 cups on
```

**Enable at multiple runlevels:**
```bash
chkconfig --level 2345 cups on
```

**Disable:**
```bash
chkconfig --level 5 cups off
```

**Verify:**
```bash
chkconfig --list cups
ls /etc/rc.d/rc5.d/S*cups
```

### systemd

**Enable (start at boot):**
```bash
systemctl enable cups.service
```

**Output:**
```
Created symlink /etc/systemd/system/printer.target.wants/cups.service
→ /usr/lib/systemd/system/cups.service
```

**Disable (don't start at boot):**
```bash
systemctl disable cups.service
```

**Note:** Disable doesn't stop running service!

**Mask (prevent from ever running):**
```bash
systemctl mask NetworkManager.service
```

**Unmask:**
```bash
systemctl unmask NetworkManager.service
```

**Check if enabled:**
```bash
systemctl is-enabled cups.service
```

---

## Setting Default Runlevel/Target

### SysVinit

**Edit /etc/inittab:**
```bash
vi /etc/inittab
```

**Change line:**
```
id:3:initdefault:  # Set to runlevel 3
```

**⚠️ Never use 0 or 6!**

### systemd

**View default:**
```bash
systemctl get-default
```

**Set default:**
```bash
systemctl set-default multi-user.target
systemctl set-default graphical.target
```

**Using runlevel targets:**
```bash
systemctl set-default runlevel3.target
systemctl set-default runlevel5.target
```

---

## Changing Runlevels/Targets

### SysVinit

**View current:**
```bash
runlevel
```

**Output:**
```
N 5  # N=none (fresh boot), 5=current
5 3  # 5=previous, 3=current
```

**Change immediately:**
```bash
init 3
telinit 3  # Same as init
```

### systemd

**View current:**
```bash
runlevel  # Still works (backward compatible)
systemctl get-default
```

**Change immediately:**
```bash
systemctl isolate multi-user.target
systemctl isolate graphical.target
```

**Backward compatible:**
```bash
init 3  # Translates to: systemctl isolate multi-user.target
```

---

## Reloading systemd Configuration

**After editing unit files:**
```bash
systemctl daemon-reload
```

**When needed:**
*   Created new unit file
*   Modified existing unit file
*   Changed symbolic links

---

## Service Management Comparison

| Task | SysVinit | systemd |
|------|----------|---------|
| **Check status** | `service svc status` | `systemctl status svc.service` |
| **Start** | `service svc start` | `systemctl start svc.service` |
| **Stop** | `service svc stop` | `systemctl stop svc.service` |
| **Restart** | `service svc restart` | `systemctl restart svc.service` |
| **Reload** | `service svc reload` | `systemctl reload svc.service` |
| **Enable** | `chkconfig --level 3 svc on` | `systemctl enable svc.service` |
| **Disable** | `chkconfig --level 3 svc off` | `systemctl disable svc.service` |
| **List all** | `chkconfig --list` | `systemctl list-unit-files --type=service` |
| **Default runlevel** | Edit `/etc/inittab` | `systemctl set-default target` |
| **Change runlevel** | `init 3` | `systemctl isolate multi-user.target` |

---

## Best Practices

**Always check status after changes:**
```bash
systemctl status service  # Don't assume success!
```

**Use reload instead of restart when possible:**
*   Doesn't interrupt service
*   No downtime
*   Pending operations complete

**Mask services you never want to run:**
```bash
systemctl mask service  # Prevents accidental start
```

**Enable vs Start:**
*   **Enable** - Start at boot (persistent)
*   **Start** - Start now (temporary)
*   Often need both!

---

## Review Questions

1. What's the difference between start and enable?

2. How do you reload a service configuration without stopping it?

3. What does systemctl mask do?

4. True or False: Disabling a service stops it immediately.

5. What command checks if a service is enabled?

6. How do you change to runlevel 3 immediately?

7. What does systemctl daemon-reload do?

8. What's a conditional restart?

---

## Quick Reference

```
SysVinit:
├── service svc status/start/stop/restart/reload
├── chkconfig --list
├── chkconfig --level 3 svc on/off
├── runlevel
└── init 3

systemd:
├── systemctl status/start/stop/restart/reload svc.service
├── systemctl enable/disable/mask/unmask svc.service
├── systemctl is-enabled svc.service
├── systemctl list-unit-files --type=service
├── systemctl get-default
├── systemctl set-default target
├── systemctl isolate target
├── systemctl daemon-reload
└── systemctl condrestart svc.service

Enable vs Start:
├── enable  → Start at boot (persistent)
└── start   → Start now (immediate)

Reload vs Restart:
├── reload  → Reload config (no stop)
└── restart → Stop then start

Disable vs Mask:
├── disable → Don't start at boot
└── mask    → Prevent from ever running
```
