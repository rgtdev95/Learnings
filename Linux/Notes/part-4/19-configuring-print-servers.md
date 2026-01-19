# Configuring Print Servers

## Sharing CUPS Printers

**Purpose:** Make local printers available to network clients

**Requirements:**
*   TCP/IP network connection
*   Firewall port 631 open
*   Proper permissions configured

---

## Configuring cupsd.conf

**Location:** `/etc/cups/cupsd.conf`

**Format:** Similar to Apache httpd.conf

### Basic Server Settings

**Listen directive:**
```bash
Listen *:631                # All interfaces
Listen localhost:631        # Local only (default)
Listen 192.168.1.100:631   # Specific IP
```

**Browsing settings:**
```bash
Browsing On
BrowseProtocols cups
BrowseOrder Deny,Allow
BrowseAllow from @LOCAL
BrowseAddress 255.255.255.255
```

**Classification (security labels):**
```bash
Classification topsecret
```

Options: `classified`, `confidential`, `secret`, `unclassified`

### Access Control

**Location blocks:**
```bash
<Location /printers/hp-printer>
  Order Deny,Allow
  Deny From All
  Allow From 127.0.0.1
  Allow From 192.168.1.0/24
  AuthType None
</Location>
```

**Administration access:**
```bash
<Location /admin>
  Order Deny,Allow
  Deny From All
  Allow From 127.0.0.1
  AuthType Basic
</Location>
```

---

## Sharing via Print Settings Window

**Steps (Fedora/RHEL):**

1. **Open Print Settings**
   ```bash
   system-config-printer
   ```

2. **Enable server sharing:**
   - Select Server → Settings
   - Check "Publish shared printers connected to this system"
   - Click OK

3. **Share specific printer:**
   - Double-click printer
   - Select Policies tab
   - Check "Shared"

4. **Configure access control:**
   - Select Access Control tab
   - Choose:
     - Allow all except listed users
     - Deny all except listed users

---

## Manually Configuring Shared Printer

**Edit `/etc/cups/printers.conf`:**

```bash
<DefaultPrinter printer>
  Info HP LaserJet 2100M
  Location Room 205
  DeviceURI parallel:/dev/lp0
  State Idle
  Accepting Yes
  Shared Yes              # Enable sharing
  JobSheets none none
  QuotaPeriod 0
  PageLimit 0
  KLimit 0
</Printer>
```

**Key setting:** `Shared Yes`

---

## Starting CUPS Server

### RHEL 6 and Earlier

**Enable at boot:**
```bash
chkconfig cups on
```

**Start service:**
```bash
service cups start
```

**Restart service:**
```bash
service cups restart
```

**Reload configuration:**
```bash
service cups reload
```

### RHEL 7/8 and Fedora

**Enable at boot:**
```bash
systemctl enable cups.service
```

**Start service:**
```bash
systemctl start cups.service
```

**Check status:**
```bash
systemctl status cups.service
```

**Restart:**
```bash
systemctl restart cups.service
```

**Reload config:**
```bash
systemctl reload cups.service
```

---

## Samba Printer Sharing

**Purpose:** Share Linux printers with Windows clients

**Configuration file:** `/etc/samba/smb.conf`

### Global Settings

```bash
[global]
...
load printers = yes
cups options = raw
printcap name = /etc/printcap
printing = cups
...
```

### Printers Share

```bash
[printers]
  comment = All Printers
  path = /var/spool/samba
  browseable = yes
  writeable = no
  printable = yes
```

**Options:**
*   `load printers = yes` - Load all CUPS printers
*   `cups options = raw` - Don't format (client provides driver)
*   `browseable = yes` - Visible to Windows clients
*   `printable = yes` - Allow printing

### Printer Driver Share

**Optional - Store Windows drivers:**

```bash
[print$]
  comment = Printer Drivers
  path = /var/lib/samba/drivers
  browseable = yes
  guest ok = no
  read only = yes
  write list = chris, admin
```

**Usage:**
*   Store Windows print drivers
*   Auto-download to clients
*   No manual driver installation needed

---

## Windows Client Setup

### Windows 10

1. Click Start → Printers and Scanners
2. Select printer from list
3. Configure

### Windows Vista/7/8

1. Open Network icon
2. Find Linux server (NetBIOS name)
3. Open server icon
4. See shared printers
5. Double-click printer
6. Click Yes to configure
7. Add Printer Wizard appears
8. Install drivers

### Windows XP

1. Start → Printers and Faxes
2. Click "Add a Printer"
3. Select "Network Printer"
4. Browse or enter path
5. Install drivers

**Alternative - Search:**
1. Start → Search → Computer
2. Enter computer name
3. Click Search
4. Double-click computer
5. Configure printer

---

## Troubleshooting Print Servers

### Firewall Issues

**Check if port 631 is open:**
```bash
netstat -tupln | grep 631
```

**Expected output:**
```
tcp 0 0 0.0.0.0:631  0.0.0.0:*  LISTEN  6492/cupsd
```

**Open firewall (Fedora/RHEL 7+):**
```bash
firewall-cmd --permanent --add-service=ipp
firewall-cmd --reload
```

**Open firewall (RHEL 6):**

Add to `/etc/sysconfig/iptables`:
```bash
-A INPUT -m state --state NEW -m tcp -p tcp --dport 631 -j ACCEPT
```

Restart:
```bash
service iptables restart
```

### Name Resolution

**Test with IP address:**
```bash
http://192.168.1.100:631
```

**If IP works but hostname doesn't:**
*   DNS problem
*   Check `/etc/hosts`
*   Verify DNS server

### cupsd Listening Address

**Check listening addresses:**
```bash
netstat -tupln | grep 631
```

**Local only:**
```
tcp 0 0 127.0.0.1:631  0.0.0.0:*  LISTEN
```

**All interfaces:**
```
tcp 0 0 0.0.0.0:631    0.0.0.0:*  LISTEN
```

**Fix:** Change `Listen` directive in cupsd.conf

### Access Denied

**Check cupsd.conf Location blocks:**
*   Verify Allow/Deny rules
*   Check IP addresses/networks
*   Verify AuthType settings

**Check printers.conf:**
*   Verify `Shared Yes`
*   Check access permissions

---

## Review Questions

1. What file is the main CUPS server configuration?

2. What port must be open for CUPS printing?

3. How do you enable printer sharing in Print Settings window?

4. What does `Shared Yes` do in printers.conf?

5. What Samba setting loads CUPS printers?

6. True or False: Windows clients need Linux drivers.

7. How do you check if cupsd is listening on all interfaces?

8. What command reloads CUPS configuration without restarting?

---

## Quick Reference

```
cupsd.conf:
└── /etc/cups/cupsd.conf

Sharing Settings:
├── Listen *:631              → Listen all interfaces
├── Browsing On               → Enable browsing
└── BrowseAllow from @LOCAL   → Allow local network

Location Blocks:
<Location /printers/name>
├── Order Deny,Allow
├── Deny From All
├── Allow From 192.168.1.0/24
└── AuthType None
</Location>

Share Printer (GUI):
1. Server → Settings
2. Check "Publish shared printers"
3. Double-click printer
4. Policies → Check "Shared"

Share Printer (Manual):
└── Edit /etc/cups/printers.conf: Shared Yes

Start CUPS (RHEL 7+):
├── systemctl enable cups.service
├── systemctl start cups.service
└── systemctl reload cups.service

Start CUPS (RHEL 6):
├── chkconfig cups on
├── service cups start
└── service cups reload

Samba Printing:
[global]
├── load printers = yes
├── cups options = raw
└── printing = cups

[printers]
├── browseable = yes
├── printable = yes
└── path = /var/spool/samba

Troubleshooting:
├── netstat -tupln | grep 631  → Check listening
├── firewall-cmd --add-service=ipp → Open firewall
├── Check cupsd.conf Location blocks
└── Verify Shared Yes in printers.conf

Windows Clients:
├── Windows 10: Printers and Scanners
├── Windows 7/8: Network → Server → Printer
└── Windows XP: Add Printer Wizard
```
