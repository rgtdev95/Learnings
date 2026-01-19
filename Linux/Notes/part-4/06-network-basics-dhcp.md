# Network Basics and DHCP

## Automatic Network Configuration

**Modern Linux networking:** Plug in cable or connect to WiFi → automatically online!

**How it works:**
1. NetworkManager detects network interface
2. Sends DHCP request
3. Receives configuration from DHCP server
4. Applies settings locally
5. You're connected!

---

## Network Components Required

For Internet connectivity, you need:

| Component | Purpose | Example |
|-----------|---------|---------|
| **Network Interface** | Hardware connection | Ethernet NIC, WiFi card |
| **IP Address** | Your system's network ID | 192.168.1.100 |
| **Subnet Mask** | Defines network/host portions | 255.255.255.0 |
| **Gateway** | Route to external networks | 192.168.1.1 |
| **DNS Server** | Translates names to IPs | 8.8.8.8 |

---

## DHCP Process

### Step 1: Activate Network Interface

**NetworkManager:**
*   Looks for available interfaces (wired/wireless)
*   Checks which are set to start automatically
*   Default: External interfaces use DHCP

### Step 2: Request DHCP Service

**DHCP client (Linux system):**
*   Sends broadcast request on network
*   Identifies itself using MAC address
*   Asks for network configuration

### Step 3: DHCP Server Response

**DHCP server provides:**

**IP Address:**
*   From pool of available addresses
*   Or specific address for specific MAC (static DHCP)
*   Example: 192.168.0.100

**Subnet Mask:**
*   Defines network vs host portion
*   Example: 255.255.255.0
*   Network: 192.168.0, Host: 100

**Lease Time:**
*   How long IP address is valid
*   Default IPv4: 86,400 seconds (24 hours)
*   Default IPv6: 2,592,000 seconds (30 days)
*   Must renew before expiration

**DNS Server:**
*   One or more DNS server addresses
*   Translates hostnames to IP addresses
*   Example: 8.8.8.8, 8.8.4.4

**Default Gateway:**
*   Router to external networks
*   Routes packets outside local network
*   Example: 192.168.0.1

**Other Information (optional):**
*   NTP server (time synchronization)
*   Print server
*   Font server
*   IRC server

### Step 4: Update Local Settings

**Linux system applies configuration:**
*   Sets IP address on interface
*   Adds DNS servers to `/etc/resolv.conf`
*   Sets default route to gateway
*   Stores lease time for renewal

---

## IP Addresses

### IPv4 Format

**Structure:** Four octets (0-255) separated by dots

**Example:** `192.168.1.100`

**Classes:**
*   Class A: 1.0.0.0 - 126.255.255.255
*   Class B: 128.0.0.0 - 191.255.255.255
*   Class C: 192.0.0.0 - 223.255.255.255

**Private ranges (not routable on Internet):**
*   10.0.0.0 - 10.255.255.255
*   172.16.0.0 - 172.31.255.255
*   192.168.0.0 - 192.168.255.255

### IPv6 Format

**Structure:** Eight groups of four hex digits

**Example:** `2600:1700:722:a10::489`

**Advantages:**
*   Vastly more addresses
*   Better security features
*   Simplified routing

---

## Subnet Masks

**Purpose:** Define network vs host portions of IP address

**Common masks:**

| CIDR | Subnet Mask | Network Bits | Host Bits | Hosts |
|------|-------------|--------------|-----------|-------|
| /8 | 255.0.0.0 | 8 | 24 | 16,777,214 |
| /16 | 255.255.0.0 | 16 | 16 | 65,534 |
| /24 | 255.255.255.0 | 24 | 8 | 254 |

**Example:**
*   IP: 192.168.1.100
*   Mask: 255.255.255.0 (/24)
*   Network: 192.168.1.0
*   Host: 100
*   Range: 192.168.1.1 - 192.168.1.254

---

## DNS (Domain Name System)

**Purpose:** Translate hostnames to IP addresses

**Hierarchy:**
```
Root servers (.)
  ↓
Top-level domains (.com, .org, .net)
  ↓
Second-level domains (example.com)
  ↓
Subdomains (www.example.com)
```

**How it works:**
1. You type: `www.example.com`
2. System checks local cache
3. Queries DNS server
4. DNS returns: `93.184.216.34`
5. Browser connects to IP address

---

## Gateway/Router

**Purpose:** Route packets between networks

**How it works:**
*   Has interfaces on multiple networks
*   Receives packets from one network
*   Forwards to appropriate destination
*   Provides path to Internet

**Default gateway:**
*   Used when destination not on local network
*   All external traffic goes through gateway

---

## MAC Addresses

**Purpose:** Unique hardware identifier for network interface

**Format:** Six pairs of hex digits

**Example:** `e0:06:e6:83:ac:c7`

**Characteristics:**
*   Assigned by manufacturer
*   Supposed to be globally unique
*   Used by DHCP to identify clients
*   Can be used for static DHCP assignments

---

## Review Questions

1. What are the four essential components for Internet connectivity?

2. What protocol automatically assigns IP addresses?

3. What is the default IPv4 DHCP lease time?

4. What does a subnet mask define?

5. True or False: DNS translates IP addresses to hostnames.

6. What is the purpose of a default gateway?

7. What uniquely identifies a network interface card?

8. What private IP range starts with 192.168?

---

## Quick Reference

```
Network Components:
├── Network Interface → Hardware (NIC, WiFi)
├── IP Address       → System identifier
├── Subnet Mask      → Network/host division
├── Gateway          → Route to external networks
└── DNS Server       → Name resolution

DHCP Process:
1. Activate interface
2. Send DHCP request (broadcast)
3. Receive response from DHCP server
4. Apply configuration locally

DHCP Provides:
├── IP Address       → Your network ID
├── Subnet Mask      → Network definition
├── Lease Time       → How long IP is valid
├── DNS Servers      → Name resolution
├── Default Gateway  → Route to Internet
└── Other info       → NTP, print servers, etc.

IP Address Types:
├── IPv4 → 192.168.1.100 (32-bit)
└── IPv6 → 2600:1700:722:a10::489 (128-bit)

Private IP Ranges:
├── 10.0.0.0/8       → Class A
├── 172.16.0.0/12    → Class B
└── 192.168.0.0/16   → Class C

Common Subnet Masks:
├── /8  → 255.0.0.0       (16M hosts)
├── /16 → 255.255.0.0     (65K hosts)
└── /24 → 255.255.255.0   (254 hosts)
```
