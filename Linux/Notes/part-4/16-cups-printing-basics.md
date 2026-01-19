# CUPS Printing Basics

## Common UNIX Printing System (CUPS)

**Purpose:** Standard printing service for Linux and UNIX systems

**Key Features:**
*   Internet Printing Protocol (IPP) based
*   Standardized printer drivers
*   Printer classes
*   Printer browsing
*   Web-based administration
*   UNIX print commands

---

## Internet Printing Protocol (IPP)

**What is IPP?**
*   Standard for sharing printers over IP networks
*   Uses HTTP protocol for communication
*   Port 631 (TCP)

**Benefits:**
*   Printer servers and clients exchange printer information
*   Servers can broadcast printer availability
*   Clients can discover printers without configuration
*   Cross-platform compatibility

---

## CUPS Architecture

**Main Components:**

**cupsd daemon:**
*   Listens on port 631
*   Handles print requests
*   Manages print queues
*   Serves web interface

**Configuration files:**
*   `/etc/cups/cupsd.conf` - Main daemon config
*   `/etc/cups/printers.conf` - Printer definitions
*   `/etc/cups/classes.conf` - Printer classes

**Web interface:**
*   Access: `http://localhost:631`
*   Administration tasks
*   Add/remove printers
*   Manage jobs

---

## Printer Drivers

**PPD Files (PostScript Printer Description):**
*   Standardized driver format
*   Works across UNIX systems
*   Manufacturer creates once, works everywhere

**Driver locations:**
*   System drivers: `/usr/share/cups/model/`
*   Custom drivers: Can be provided as PPD files

**Driver types:**
*   PostScript - For PostScript printers
*   PCL - HP Printer Control Language
*   Raw - Pre-formatted data

---

## Printer Classes

**Purpose:** Group multiple printers or create multiple entries

**Use cases:**

**Multiple entries, one printer:**
*   Different paper trays
*   Different print quality settings
*   Different margins/formatting

**One entry, multiple printers:**
*   Load balancing
*   Redundancy
*   Prevents single point of failure

**Implicit classes:**
*   Automatically formed
*   Merges identical network printers
*   No manual configuration needed

---

## Printer Browsing

**What is browsing?**
*   Broadcasting printer information on network
*   Listening for other printers
*   Automatic printer discovery

**Benefits:**
*   Clients see available printers automatically
*   No manual configuration needed
*   Easy printer selection

**Control:**
*   Can be enabled/disabled per printer
*   Prevents unwanted printer visibility
*   Configured in cupsd.conf

**Default behavior:**
*   Enabled for local networks
*   Broadcast on 255.255.255.255

---

## UNIX Print Commands

**CUPS provides traditional UNIX commands:**

| Command | Purpose |
|---------|---------|
| `lp` | Print files |
| `lpq` | View print queue |
| `lprm` | Remove print jobs |
| `lpstat` | Show printer status |
| `lpadmin` | Administer printers |

**Compatibility:**
*   Works like traditional UNIX printing
*   Integrates into existing workflows
*   Familiar to UNIX administrators

---

## CUPS Package Contents

**Fedora/RHEL packages:**
*   `cups` - Main package (cupsd daemon)
*   `cups-client` - Client tools
*   `system-config-printer` - GUI configuration

**Installation:**
```bash
yum install cups cups-client
```

**Ubuntu/Debian:**
```bash
apt-get install cups
```

---

## Configuration Methods

**1. Web-based (localhost:631):**
*   Built-in CUPS interface
*   Works on all distributions
*   Administration tab for management

**2. GUI tools:**
*   Print Settings window (Fedora/RHEL)
*   system-config-printer
*   Desktop-specific tools

**3. Manual configuration:**
*   Edit `/etc/cups/cupsd.conf`
*   Edit `/etc/cups/printers.conf`
*   Command-line tools

---

## Printing from Windows to CUPS

**Direct printing:**
*   Use PostScript driver on Windows
*   Point to: `http://printserver:631/printers/targetPrinter`
*   No CUPS modification needed

**Raw Print Queue:**
*   For native Windows drivers
*   Passes data directly to printer
*   No translation by CUPS

---

## Review Questions

1. What port does CUPS use by default?

2. What does PPD stand for?

3. What are printer classes used for?

4. How do you access the CUPS web interface?

5. True or False: CUPS can only be configured via web interface.

6. What protocol does CUPS use for printing?

7. What is printer browsing?

8. Where are CUPS configuration files stored?

---

## Quick Reference

```
CUPS Overview:
├── Protocol: IPP (Internet Printing Protocol)
├── Port: 631 (TCP)
├── Daemon: cupsd
└── Web interface: http://localhost:631

Configuration Files:
├── /etc/cups/cupsd.conf     → Daemon config
├── /etc/cups/printers.conf  → Printer definitions
└── /etc/cups/classes.conf   → Printer classes

Key Features:
├── IPP-based printing
├── Standardized drivers (PPD)
├── Printer classes
├── Printer browsing
├── Web administration
└── UNIX command compatibility

Installation:
└── yum install cups cups-client

Print Commands:
├── lp        → Print files
├── lpq       → View queue
├── lprm      → Remove jobs
├── lpstat    → Show status
└── lpadmin   → Administer

Printer Classes:
├── Multiple entries → One printer (different settings)
└── One entry → Multiple printers (load balancing)

Browsing:
├── Broadcasts printer info
├── Auto-discovery
└── Can be disabled per printer
```
