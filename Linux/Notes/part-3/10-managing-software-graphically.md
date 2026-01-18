# Managing Software Graphically

## Software Window (GNOME)

The **Software** window provides an easy way to find and install desktop applications.

**Access:** Activities → Type "Software" → Press Enter

**Available in:** Fedora, Ubuntu, RHEL 8+

---

## Key Features

### 1. Editor's Picks

Popular, curated applications ready to install

### 2. Categories

Browse by type:
*   Audio & Video
*   Games
*   Graphics & Photography
*   Productivity
*   Development Tools
*   Utilities

### 3. Search

Find apps by name or description

### 4. Installed Tab

View all installed applications

### 5. Updates Tab

See available updates for installed apps

---

## Using the Software Window

### Installing an Application

1. **Open Software window**
2. **Search or browse** for application
3. **Click application** to see details
4. **Click Install** button
5. **Wait for download** and installation
6. **App appears** in Activities menu

**What Actually Happens:**
*   Software window uses package manager (DNF/APT)
*   Downloads application package(s)
*   Downloads dependency packages
*   Installs all packages together

### Removing an Application

1. **Go to Installed tab**
2. **Find application**
3. **Click Remove button**
4. **Confirm removal**

---

## Third-Party Repositories

**First time opening Software:**
*   Prompt to "Enable third-party repositories"
*   Includes free-to-use (but not redistributable) software
*   Not part of official Fedora/Ubuntu repos

**Decision:**
*   ✅ Enable: Access to more applications (Adobe Flash, etc.)
*   ❌ Decline: Stick to official repositories only

---

## Limitations of the Software Window

### What You Can See

*   Desktop applications with GUI
*   Games and multimedia apps
*   Productivity software

### What You CAN'T See

| Hidden From View | Examples |
|------------------|----------|
| **Command-line tools** | `wget`, `curl`, `tmux` |
| **System services** | `httpd`, `sshd`, `vsftpd` |
| **Programming libraries** | Development headers, SDKs |
| **Documentation packages** | Man pages, guides |
| **Individual components** | Extra fonts, language packs |

**Result:** Hundreds visible vs. thousands available

---

## Why Go Beyond the Software Window?

### 1. Access More Software

**Example:** Want to install a web server?

*   Software window: May not show `httpd`
*   Command line: `sudo dnf install httpd`

### 2. Fine-Grained Control

**Application Install:**
*   Installs base package + defaults
*   May include unwanted extras (fonts, docs, language packs)

**Manual Install:**
*   Choose exact packages
*   Exclude unneeded components

### 3. Complex Queries

**Software Window:**
*   Basic search by name
*   Basic package info

**Command Line:**
*   Search by file provided
*   List all dependencies
*   View change logs
*   Check file ownership

### 4. Software Validation

**Command Line:**
*   Verify package signatures
*   Check for tampered files
*   Validate checksums

**Software Window:** No validation tools

### 5. Enterprise Management

**Software Window:**
*   One system at a time
*   Manual clicks required

**Command Line + Automation:**
*   Manage hundreds of systems
*   Scripted deployments
*   Automated updates

---

## Ubuntu Software Center

**Ubuntu equivalent** of Fedora Software window

**Same features:**
*   Browse categories
*   Search applications
*   Install/remove apps
*   View updates

**Also limited** to graphical applications

---

## Other Graphical Tools (Older)

### Fedora/RHEL (Pre-GNOME Software)

*   **PackageKit** - Older graphical installer
*   **system-config-packages** - Legacy tool

### Debian/Ubuntu

**Ubuntu Software Center** (older versions)
**Synaptic Package Manager:**
*   More advanced than Software window
*   Shows all packages (not just apps)
*   Better for power users

**Install Synaptic:**
```bash
sudo apt-get install synaptic
```

---

## When to Use Each Tool

### Use Software Window When:

✅ Installing desktop applications
✅ Browsing for games/productivity apps
✅ Managing single system
✅ Prefer graphical interface
✅ Don't need advanced features

### Use Command Line When:

✅ Installing system services/daemons
✅ Need specific package versions
✅ Managing multiple systems
✅ Automating installations
✅ Troubleshooting package issues
✅ Installing from custom repositories
✅ Validating package integrity

---

## Review Questions

1. How do you access the Software window in GNOME?

2. What types of software are visible in the Software window?

3. Name three types of packages NOT shown in the Software window.

4. True or False: The Software window can validate package signatures.

5. What is the Ubuntu equivalent of Fedora's Software window?

6. Why might you use command-line tools instead of the Software window?

7. What happens when you enable third-party repositories?

8. Which graphical tool shows ALL packages in Ubuntu?

---

## Quick Reference

```
Software Window:
├── Access: Activities → "Software"
├── Tabs:
│   ├── Explore    → Browse/search apps
│   ├── Installed  → Manage installed apps
│   └── Updates    → Available updates
└── Limitations:
    ├── Desktop apps only
    ├── No CLI tools
    ├── No services/daemons
    └── Limited package control

When to Use CLI:
├── System services (httpd, sshd)
├── Development tools
├── Automation/scripting
├── Package validation
└── Enterprise management
```
