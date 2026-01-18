# Installation Methods

## Key Concepts

| Term | Definition |
|------|------------|
| **Live Media** | Bootable ISO containing a complete OS (can run without installation) |
| **Installation DVD** | ISO with packages for flexible custom installation |
| **PXE Boot** | Network boot from a PXE server |
| **Kickstart** | Automated installation using answer file |

---

## Installation Method Overview

Linux can be installed in several ways, each suited for different scenarios:

| Method | Use Case | Advantages |
|--------|----------|------------|
| **Live Media** | Simple desktop install | Easy, fast, runs before installing |
| **Installation DVD** | Custom server/desktop | Full package selection, flexible |
| **Cloud/VM** | Virtual environments | Metadata-based, quick deployment |
| **Enterprise (PXE/Kickstart)** | Mass deployment | Automated, consistent, scalable |

---

## Method 1: Live Media Installation

A **Live media ISO** is a single, read-only image containing everything to start Linux.

**Characteristics:**
*   Can run entirely from DVD/USB without touching hard disk
*   Works on systems with no hard disk
*   Simplest installation method
*   Copies entire Live system to hard disk

**Process:**
1. Boot from Live DVD/USB
2. Test the system (optional)
3. Click "Install to Hard Drive"
4. Answer minimal questions
5. Reboot to installed system

**Trade-offs:**
*   ✅ Simple and fast
*   ✅ Preview system before installing
*   ❌ Limited package selection (install what's on the ISO)
*   ❌ Larger ISO files (won't fit on CD)

---

## Method 2: Installation DVD

An **installation DVD** offers flexible, package-based installation.

**Characteristics:**
*   Software split into individual packages
*   Choose exactly which packages to install
*   Multiple installation types (minimal, server, desktop, workstation)
*   Can add custom repositories during installation

**Installation Types:**
*   **Minimal Install:** Bare-bones system (~600MB)
*   **Server:** No GUI, server packages
*   **Server with GUI:** GNOME desktop + server tools
*   **Workstation:** Desktop-focused setup

**Trade-offs:**
*   ✅ Full package control
*   ✅ Multiple environments available
*   ✅ Smaller base ISO
*   ❌ More installation questions
*   ❌ Takes longer than Live install

---

## Method 3: Cloud-Based Installation

Cloud installations use **prebuilt images** with **metadata** for configuration.

**How It Works:**
1. Start with base OS image (contains complete Linux system)
2. Add metadata (hostname, password, network, disk, CPU, RAM)
3. Launch as virtual machine
4. System boots with applied configuration

**Cloud Platforms:**
*   Amazon EC2
*   Google Compute Engine
*   OpenStack
*   Azure
*   KVM (local virtualization)

**Key Difference:** No traditional "installer" - physical hardware is abstracted into resource pools.

---

## Method 4: Enterprise Installation

For deploying **dozens to thousands** of systems, enterprise methods automate installation.

### Components

**PXE Boot Server:**
*   Computers boot from network (NIC with PXE support)
*   PXE server provides kernel and initial RAM disk
*   No physical media needed

**Installation Server:**
*   Hosts software packages
*   Accessible via HTTP, FTP, or NFS
*   One server can serve many installations

**Kickstart Files:**
*   Plain-text answer file
*   Contains all installation choices
*   Can include custom scripts (pre/post install)
*   Location passed as boot option: `ks=http://server/ks.cfg`

### Workflow
```
1. Computer boots → PXE NIC
2. PXE server → Sends kernel + initrd
3. Kernel boots → Reads kickstart location
4. Anaconda installer → Gets packages from install server
5. Kickstart scripts → Configure installed system
6. System reboots → Fully configured
```

---

## Hardware Requirements

### Minimum Requirements (RHEL/Fedora Desktop)

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **CPU** | 1GHz Pentium | 2GHz+ (64-bit for virtualization) |
| **RAM** | 1GB | 2-3GB+ |
| **Disk** | 20GB (avg desktop) | Depends on use case |
| **Boot Media** | DVD or USB drive | — |
| **Network** | Wired/wireless NIC | For updates/repos |

### Special Requirements

*   **Virtualization Host (KVM):** Requires AMD-V or Intel-VT CPU
*   **Lightweight Systems:** Can run on 24MB RAM, 486 CPU (e.g., Tiny Core Linux)

---

## Installation Destination Options

| Option | Description |
|--------|-------------|
| **Single-Boot** | Linux only, erases entire disk |
| **Multi-Boot** | Linux + Windows, select OS at boot |
| **Virtual Machine** | Install in KVM, VirtualBox, VMware, Hyper-V |
| **Bare Metal** | Direct hardware installation |

---

## Review Questions

1. What is the main advantage of Live media installation?

2. Which installation method gives you the most control over packages?

3. What are the three main components of enterprise Linux installation?

4. What does PXE stand for?

5. True or False: Kickstart files can contain custom scripts.

6. What metadata is typically added to a cloud image?

7. What CPU feature is required for KVM virtualization?

8. Which installation method is best for deploying 100 identical systems?

---

## Quick Reference

```
Installation Methods:
├── Live Media      → Simple, fast, preview system
├── Installation DVD → Full control, package selection
├── Cloud/VM        → Image + metadata deployment
└── Enterprise      → PXE + kickstart automation

Hardware Check:
├── CPU: 1GHz+ (64-bit for virt)
├── RAM: 2-3GB recommended
└── Disk: 20GB+ for desktop
```

