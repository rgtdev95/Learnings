# Setting Up Printers

## Automatic Printer Detection

**USB printers:**
*   Automatically detected when connected
*   Driver installed automatically (if available)
*   Ready to use immediately
*   Appears in Print Settings

**Network printers:**
*   CUPS can broadcast availability
*   Clients detect automatically
*   No manual configuration needed
*   Works with printer browsing enabled

---

## Web-Based CUPS Administration

**Accessing CUPS web interface:**
```bash
http://localhost:631
```

**From remote systems:**
```bash
http://hostname:631
```

### Enabling Remote Administration

**Steps:**
1. Select Administration tab
2. Check "Allow remote administration"
3. Click "Change Settings"
4. Open firewall port 631
5. Restart CUPS: `systemctl restart cups.service`

**Authentication:**
*   Root username required
*   Root password required
*   Prompted when needed

---

## Adding a Printer (Web Interface)

**Steps:**

1. **Click "Add Printer"** on Administration page

2. **Select device:**
   - Local: Parallel, SCSI, Serial, USB
   - Network: AppSocket, HP JetDirect, IPP, SMB

3. **Provide connection details:**
   - Network address (for network printers)
   - URI format varies by type

4. **Name the printer:**
   - Printer name (letters, numbers, dashes, underscores)
   - Location description
   - Human-readable description
   - Share option

5. **Select driver:**
   - Choose manufacturer
   - Select model
   - Or provide PPD file

6. **Set options:**
   - Paper size
   - Print quality
   - Other printer-specific settings

7. **Test:**
   - Print test page
   - Verify output

---

## Print Settings Window (Fedora/RHEL)

**Installation:**
```bash
yum install system-config-printer
```

**Launch:**
```bash
system-config-printer
```

Or: Type "Print Settings" in Activities screen

### Adding a Local Printer

**Procedure:**

1. **Open Print Settings window**
   ```bash
   system-config-printer &
   ```

2. **Click Add**
   - May prompt to adjust firewall

3. **Select device:**
   - Detected printers shown automatically
   - Or choose: LPT #1, Serial Port #1, USB

4. **Choose driver:**
   - Select from database
   - Or provide PPD file

5. **Configure printer:**
   - Name (must start with letter)
   - Description
   - Location

6. **Test:**
   - Print test page
   - Verify with `lp /etc/hosts`

---

## Configuring Remote Printers

### Network Printer Types

**CUPS (IPP) Printer:**
*   Most Linux/Mac printers
*   Port 631
*   IPP protocol

**UNIX (LPD/LPR) Printer:**
*   Traditional UNIX printing
*   Port 515
*   LPD protocol

**Windows (SMB) Printer:**
*   Samba/Windows sharing
*   Port 139/445
*   SMB protocol

**HP JetDirect:**
*   Network-attached HP printers
*   Port 9100
*   AppSocket protocol

---

## Adding Remote CUPS Printer

**Required information:**

**Host:**
*   Hostname or IP address
*   Example: `printserver.example.com`
*   Or: `192.168.1.100`

**Queue:**
*   Printer name on remote server
*   Example: `hp-laserjet`
*   With instance: `hp/300dpi`

**Format:**
```
ipp://hostname:631/printers/queuename
```

---

## Adding Remote UNIX (LPD) Printer

**Required information:**

**Host:**
*   Hostname or IP address
*   Can probe for host

**Queue:**
*   Printer name on remote system

**Format:**
```
lpd://hostname/queuename
```

**Troubleshooting:**
*   Check `/etc/lpd.perms` on server
*   Verify hostname access
*   Use `lpstat -d printer` to check status

---

## Adding Windows (SMB) Printer

**URI format:**
```
smb://[workgroup/]server/printer
```

**With authentication:**
```
smb://username:password@workgroup/server/printer
```

**Example:**
```
smb://jjones:mypass@OFFICE/SERVER1/HP-LaserJet
```

**Components:**
*   **Workgroup:** Windows workgroup name (optional)
*   **Server:** NetBIOS name or IP address
*   **Share:** Printer share name
*   **User:** Username for access
*   **Password:** User password

**⚠️ Security Warning:**
*   Credentials stored in `/etc/cups/printers.conf`
*   Stored unencrypted
*   File should be readable only by root

**Authentication options:**
*   Prompt user when needed
*   Set authentication details now

---

## Editing Printer Properties

**Settings tab:**
*   Description
*   Location
*   Device URI
*   Make and Model

**Policies tab:**
*   State (Enabled, Accepting Jobs, Shared)
*   Error handling (stop-printer, abort-job, retry-job)
*   Banner pages (Classified, Confidential, Secret)

**Access Control:**
*   Allow all except listed users
*   Deny all except listed users

**Printer Options:**
*   Watermark settings
*   Resolution enhancement
*   Page size
*   Media source (paper tray)
*   Gray levels
*   Resolution (DPI)
*   EconoMode

**Job Options:**
*   Copies, orientation, scale
*   Pages per side
*   Image options (scaling, saturation, hue, gamma)
*   Text options (characters/inch, lines/inch, margins)

**Ink/Toner Levels:**
*   View remaining ink/toner
*   (Not all printers support this)

---

## Setting Default Printer

**From Print Settings:**
1. Right-click printer
2. Select "Set As Default"

**From command line:**
```bash
lpadmin -d printername
```

---

## Review Questions

1. What port does CUPS web interface use?

2. How do you enable remote CUPS administration?

3. What package provides the Print Settings window in Fedora?

4. What are the three main types of remote printers?

5. True or False: SMB printer credentials are stored encrypted.

6. What command launches the Print Settings window?

7. What is the format for an IPP printer URI?

8. How do you set a default printer from the GUI?

---

## Quick Reference

```
Web Interface:
└── http://localhost:631

Remote Administration:
1. Administration tab
2. Check "Allow remote administration"
3. Open firewall port 631
4. Restart CUPS

Print Settings Window:
├── Install: yum install system-config-printer
└── Launch: system-config-printer

Adding Local Printer:
1. Click Add
2. Select device
3. Choose driver
4. Configure name/location
5. Test

Remote Printer Types:
├── CUPS (IPP)    → ipp://host:631/printers/queue
├── UNIX (LPD)    → lpd://host/queue
├── Windows (SMB) → smb://user:pass@workgroup/server/printer
└── JetDirect     → socket://host:9100

Printer Properties:
├── Settings      → Description, location, URI
├── Policies      → State, error handling, banners
├── Access Control → Allow/deny users
├── Printer Options → Watermark, resolution, paper
└── Job Options   → Copies, orientation, margins

Set Default:
├── GUI: Right-click → Set As Default
└── CLI: lpadmin -d printername
```
