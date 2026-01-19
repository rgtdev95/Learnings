# systemd Service Management

## systemd Unit Types

**12 unit types:**
1. **service** - Daemon management
2. **target** - Group of units
3. **socket** - Socket activation
4. **device** - Device management
5. **mount** - Mount point management
6. **automount** - Auto-mount points
7. **swap** - Swap space
8. **path** - Path-based activation
9. **timer** - Scheduled tasks
10. **snapshot** - System state snapshot
11. **slice** - Resource management
12. **scope** - External processes

**Most important:** service and target units

---

## Service Unit Files

**Locations:**
*   `/lib/systemd/system/` - System defaults
*   `/etc/systemd/system/` - Local customizations (takes precedence)

**List service units:**
```bash
systemctl list-unit-files --type=service
```

**Output:**
```
UNIT FILE           STATE
cups.service        enabled
sshd.service        enabled
NetworkManager.service enabled
```

**States:**
*   **enabled** - Starts at boot
*   **disabled** - Doesn't start at boot
*   **static** - Enabled by default, can't disable

---

## Service Unit Configuration

**Example:** `/lib/systemd/system/sshd.service`

```ini
[Unit]
Description=OpenSSH server daemon
After=network.target sshd-keygen.target

[Service]
Type=notify
EnvironmentFile=-/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Key options:**
*   `Description` - Service description
*   `After` - Start after these units
*   `ExecStart` - Command to start service
*   `ExecReload` - Command to reload
*   `WantedBy` - Target that wants this service

---

## Target Units

**Purpose:** Group related services

**Common targets:**
*   `poweroff.target` - Shutdown
*   `rescue.target` - Single user
*   `multi-user.target` - CLI multiuser
*   `graphical.target` - GUI multiuser
*   `reboot.target` - Reboot

**List target units:**
```bash
systemctl list-unit-files --type=target
```

**View target's services:**
```bash
systemctl show --property "Wants" multi-user.target
```

**Better formatted:**
```bash
systemctl show --property "Wants" multi-user.target | \
  fmt -10 | sed 's/Wants=//g' | sort
```

---

## Target Unit Configuration

**Example:** `/lib/systemd/system/multi-user.target`

```ini
[Unit]
Description=Multi-User
Requires=basic.target
Conflicts=rescue.service rescue.target
After=basic.target rescue.service rescue.target
AllowIsolate=yes
```

**Key options:**
*   `Requires` - Must activate these (or fail)
*   `Wants` - Try to activate these (optional)
*   `Conflicts` - Stop these when starting
*   `After` - Ordering (start after these)
*   `AllowIsolate` - Can be isolated (switched to)

---

## systemctl Command

**Purpose:** Manage systemd units

**List units:**
```bash
systemctl list-units
systemctl list-units --type=service
systemctl list-units --type=target
```

**Check status:**
```bash
systemctl status cups.service
```

**Output:**
```
cups.service - CUPS Printing Service
   Loaded: loaded (/lib/systemd/system/cups.service; enabled)
   Active: active (running) since Wed 2019-09-18 17:32:27
 Main PID: 874 (cupsd)
   Status: "Scheduler is running..."
```

---

## Wants vs Requires

**Wants (optional):**
*   Units listed are started
*   If they fail, no problem
*   Target continues

**Requires (mandatory):**
*   Units listed must start
*   If they fail, entire unit fails
*   More strict, potentially catastrophic

**View Requires:**
```bash
systemctl show --property "Requires" multi-user.target
```

---

## Default Target

**Check default:**
```bash
systemctl get-default
```

**Set default:**
```bash
systemctl set-default multi-user.target
systemctl set-default graphical.target
```

**Symbolic link:**
```bash
ls -l /etc/systemd/system/default.target
lrwxrwxrwx ... default.target -> /lib/systemd/system/graphical.target
```

---

## Backward Compatibility

**Runlevel targets exist:**
```bash
ls -l /lib/systemd/system/runlevel*.target
runlevel0.target -> poweroff.target
runlevel1.target -> rescue.target
runlevel2.target -> multi-user.target
runlevel3.target -> multi-user.target
runlevel4.target -> multi-user.target
runlevel5.target -> graphical.target
runlevel6.target -> reboot.target
```

**Old commands work:**
```bash
init 3  # Translates to: systemctl isolate multi-user.target
runlevel  # Still shows runlevel (but discouraged)
```

**/etc/inittab:**
*   Still exists
*   Contains only comments
*   No functional use

---

## Review Questions

1. What are the two most important systemd unit types?

2. Where are system service unit files stored?

3. What does WantedBy mean in a service unit?

4. What's the difference between Wants and Requires?

5. True or False: static services can be disabled.

6. What command shows the default target?

7. What does systemctl isolate do?

8. Where should custom service units be stored?

---

## Quick Reference

```
Unit Files:
├── /lib/systemd/system/  → System defaults
└── /etc/systemd/system/  → Local (takes precedence)

systemctl Commands:
├── systemctl list-units              → List all
├── systemctl list-unit-files --type=service → Services
├── systemctl status service          → Check status
├── systemctl show --property "Wants" target → View wants
└── systemctl get-default             → Show default target

Service States:
├── enabled  → Starts at boot
├── disabled → Doesn't start at boot
└── static   → Enabled by default, can't disable

Target Units:
├── poweroff.target    → Shutdown
├── rescue.target      → Single user
├── multi-user.target  → CLI multiuser
├── graphical.target   → GUI multiuser
└── reboot.target      → Reboot

Unit File Sections:
├── [Unit]    → Description, dependencies
├── [Service] → How to start/stop
└── [Install] → Where to install (WantedBy)

Dependencies:
├── Wants    → Optional (failure OK)
├── Requires → Mandatory (failure = fail)
└── After    → Ordering

Default Target:
├── systemctl get-default       → View
└── systemctl set-default target → Set
```
