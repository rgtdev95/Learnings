# Adding Custom Services

## Adding Services to SysVinit

### Step 1: Create Service Script

**Template structure:**
```bash
#!/bin/bash
# chkconfig: 2345 25 10
# description: My custom service

# Source function library
. /etc/rc.d/init.d/functions

DAEMON=/usr/bin/my_service
PROG=my_service

start() {
    echo -n $"Starting $PROG: "
    daemon $DAEMON
    RETVAL=$?
    echo
    [ $RETVAL = 0 ] && touch /var/lock/subsys/$PROG
    return $RETVAL
}

stop() {
    echo -n $"Stopping $PROG: "
    killproc $DAEMON
    RETVAL=$?
    echo
    [ $RETVAL = 0 ] && rm -f /var/lock/subsys/$PROG
    return $RETVAL
}

restart() {
    stop
    start
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    restart)
        restart
        ;;
    status)
        status $DAEMON
        RETVAL=$?
        ;;
    *)
        echo $"Usage: $0 {start|stop|restart|status}"
        RETVAL=1
esac

exit $RETVAL
```

**chkconfig line explained:**
```bash
# chkconfig: 2345 25 10
#            ^^^^  ^^ ^^
#            |     |  |
#            |     |  +-- Kill order (10)
#            |     +----- Start order (25)
#            +----------- Runlevels (2,3,4,5)
```

### Step 2: Install Script

**Copy to init.d:**
```bash
cp My_New_Service /etc/rc.d/init.d/
```

**Set permissions:**
```bash
chmod 755 /etc/rc.d/init.d/My_New_Service
```

### Step 3: Add to Runlevels

**Add service:**
```bash
chkconfig --add My_New_Service
```

**Verify:**
```bash
ls /etc/rc?.d/*My_New_Service
chkconfig --list My_New_Service
```

### Step 4: Test

**Start service:**
```bash
service My_New_Service start
```

**Check status:**
```bash
service My_New_Service status
```

**Stop service:**
```bash
service My_New_Service stop
```

---

## Adding Services to systemd

### Step 1: Create Unit File

**Basic template:**
```ini
[Unit]
Description=My New Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/my_service
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Service types:**
*   `simple` - Main process (default)
*   `forking` - Forks child process
*   `oneshot` - Runs once and exits
*   `notify` - Sends notification when ready
*   `dbus` - Acquires D-Bus name

**Advanced example:**
```ini
[Unit]
Description=My Advanced Service
Documentation=man:my_service(8)
After=network.target syslog.target
Requires=network.target

[Service]
Type=forking
User=myuser
Group=mygroup
EnvironmentFile=-/etc/sysconfig/my_service
ExecStartPre=/usr/bin/my_service_check
ExecStart=/usr/sbin/my_service -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/bin/kill -TERM $MAINPID
PIDFile=/var/run/my_service.pid
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

**Common options:**
*   `Description` - Service description
*   `After` - Start after these units
*   `Requires` - Must have these units
*   `ExecStart` - Start command
*   `ExecStop` - Stop command
*   `ExecReload` - Reload command
*   `Restart` - Restart policy
*   `User/Group` - Run as user/group
*   `WantedBy` - Target that wants this

### Step 2: Install Unit File

**Choose location:**

**System default (overwritten by updates):**
```bash
/lib/systemd/system/my_service.service
```

**Local custom (preferred):**
```bash
/etc/systemd/system/my_service.service
```

**Copy file:**
```bash
cp my_service.service /etc/systemd/system/
```

**Set permissions:**
```bash
chmod 644 /etc/systemd/system/my_service.service
```

**Reload systemd:**
```bash
systemctl daemon-reload
```

### Step 3: Add to Target (Optional)

**Method 1: Enable command (recommended):**
```bash
systemctl enable my_service.service
```

**Method 2: Manual symbolic link:**
```bash
ln -s /etc/systemd/system/my_service.service \
      /etc/systemd/system/multi-user.target.wants/my_service.service
```

### Step 4: Test

**Start service:**
```bash
systemctl start my_service.service
```

**Check status:**
```bash
systemctl status my_service.service
```

**Stop service:**
```bash
systemctl stop my_service.service
```

**Enable at boot:**
```bash
systemctl enable my_service.service
```

---

## Customizing Existing Services

### SysVinit

**Copy original:**
```bash
cp /etc/rc.d/init.d/httpd /etc/rc.d/init.d/httpd.custom
```

**Edit as needed:**
```bash
vi /etc/rc.d/init.d/httpd.custom
```

**Add to runlevels:**
```bash
chkconfig --add httpd.custom
```

### systemd

**Copy to /etc:**
```bash
cp /lib/systemd/system/httpd.service \
   /etc/systemd/system/httpd.service
```

**Edit:**
```bash
vi /etc/systemd/system/httpd.service
```

**Reload:**
```bash
systemctl daemon-reload
```

**Note:** File in `/etc` takes precedence over `/lib`

---

## Troubleshooting

**SysVinit:**
*   Check script syntax: `bash -n /etc/rc.d/init.d/service`
*   Test manually: `/etc/rc.d/init.d/service start`
*   Check logs: `/var/log/messages`

**systemd:**
*   Check syntax: `systemd-analyze verify service.service`
*   View logs: `journalctl -u service.service`
*   Debug: `systemctl status service.service -l`
*   Check dependencies: `systemctl list-dependencies service.service`

---

## Review Questions

1. What does the chkconfig line in a SysVinit script define?

2. Where should custom systemd unit files be stored?

3. What command reloads systemd after editing unit files?

4. What's the difference between Type=simple and Type=forking?

5. True or False: Files in /etc/systemd/system/ override /lib/systemd/system/.

6. What does WantedBy mean in a systemd unit file?

7. How do you test a SysVinit script for syntax errors?

8. What command shows systemd service dependencies?

---

## Quick Reference

```
SysVinit Script:
1. Create script in /etc/rc.d/init.d/
2. chmod 755 script
3. chkconfig --add service
4. service service start

systemd Unit File:
1. Create .service file
2. Copy to /etc/systemd/system/
3. chmod 644 file
4. systemctl daemon-reload
5. systemctl enable service
6. systemctl start service

Unit File Locations:
├── /lib/systemd/system/  → System (overwritten)
└── /etc/systemd/system/  → Custom (preferred)

Service Types:
├── simple   → Main process
├── forking  → Forks child
├── oneshot  → Runs once
├── notify   → Sends notification
└── dbus     → D-Bus name

Troubleshooting:
SysVinit:
├── bash -n script
└── /var/log/messages

systemd:
├── systemd-analyze verify unit
├── journalctl -u service
├── systemctl status service -l
└── systemctl list-dependencies service
```
