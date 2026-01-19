# Configuring Network Interfaces

## Setting Static IP Addresses

**Via NetworkManager GUI:**

1. Settings → Network
2. Select interface → Settings (gear icon)
3. IPv4 tab
4. Change "Automatic (DHCP)" to "Manual"
5. Fill in:
   - **Address:** 192.168.100.100
   - **Netmask:** 255.255.255.0
   - **Gateway:** 192.168.100.1
   - **DNS:** 8.8.8.8, 8.8.4.4
6. Click Apply

**Result:** Network restarts with new static settings

---

## Setting IP Address Aliases

**Purpose:** Multiple IP addresses on single NIC

**Use cases:**
*   Web server with multiple SSL certificates
*   Testing different configurations
*   Virtual hosting

**Via NetworkManager:**
1. Same screen as static IP
2. Add second address in "Addresses" section
3. Fill in IP and netmask
4. Gateway not required for alias
5. Click Apply

**Example aliases:**
*   Primary: 192.168.100.100/24
*   Alias 1: 192.168.100.103/24
*   Alias 2: 192.168.99.1/24

**Verify:**
```bash
ip addr show enp4s0
```

**Output shows:**
```
inet 192.168.100.100/24 scope global enp4s0
inet 192.168.100.103/24 scope global secondary enp4s0
```

---

## Setting Custom Routes

**Purpose:** Direct traffic to specific networks via specific gateways

**When needed:**
*   Multiple routers on network
*   Access to private networks
*   VPN connections

**Via NetworkManager:**
1. Settings → Network → Interface → Settings
2. Scroll to "Routes" section
3. Click "Add"
4. Fill in:
   - **Address:** 192.168.200.0 (destination network)
   - **Netmask:** 255.255.255.0
   - **Gateway:** 192.168.1.199 (router to that network)
5. Click Apply

**Verify:**
```bash
route -n
```

**Output:**
```
Destination     Gateway         Genmask         Iface
0.0.0.0         192.168.100.1   0.0.0.0         p4p1
192.168.100.0   0.0.0.0         255.255.255.0   p4p1
192.168.200.0   192.168.1.199   255.255.255.0   p4p1
```

---

## Routing Logic

**Packet destination determines route:**

**Local system:**
*   Sent to `lo` (loopback) interface
*   Address: 127.0.0.1

**Local network:**
*   Sent directly through NIC
*   No gateway needed

**Custom route:**
*   Matches specific network
*   Sent to designated gateway

**Default route:**
*   Everything else
*   Sent to default gateway (Internet)

---

## Proxy Server Configuration

**Purpose:** Access Internet through firewall

**Proxy types:**
*   HTTP/HTTPS - Web traffic
*   FTP - File transfer
*   SOCKS - Multiple protocols

### Via NetworkManager

1. Settings → Network → Network Proxy
2. Choose configuration method:
   - Automatic
   - Manual
   - Use system proxy settings
3. For manual, fill in:
   - HTTP Proxy: 192.168.1.1
   - Port: 3128
   - Use for all protocols (optional)
   - No Proxy for: localhost, 127.0.0.1

### Via Firefox

1. Firefox → Preferences
2. Network Settings → Settings
3. Choose:
   - Auto-detect proxy settings
   - Use system proxy settings
   - Manual proxy configuration
4. For manual:
   - HTTP Proxy: 192.168.1.1
   - Port: 3128
   - ☑ Use this proxy for all protocols
   - No Proxy for: localhost, 127.0.0.1

---

## Network Configuration with nmtui

**Purpose:** Text-based menu interface for servers

**Install:**
```bash
yum install NetworkManager-tui
```

**Launch:**
```bash
nmtui
```

**Options:**
*   Edit a connection
*   Activate a connection
*   Set system hostname

### Editing Connection

1. Select "Edit a connection"
2. Choose interface
3. IPv4 Configuration → Show
4. Change "Automatic" to "Manual"
5. Fill in:
   - Addresses: 192.168.0.150/24
   - Gateway: 192.168.0.1
   - DNS servers: 8.8.8.8
   - Search domains: example.com
6. Tab to OK → Enter
7. Quit

**Custom routes in nmtui:**
*   Routing → Edit
*   Destination/Prefix: 192.168.200.0/24
*   Next Hop: 192.168.1.199
*   OK

---

## Review Questions

1. How do you set a static IP address in NetworkManager?

2. What is an IP address alias?

3. When would you use a custom route?

4. What is the default proxy port?

5. True or False: An alias requires a gateway setting.

6. What command verifies custom routes?

7. What tool provides text-based network configuration?

8. What does a proxy server do?

---

## Quick Reference

```
Static IP (NetworkManager):
1. Settings → Network
2. Interface → Settings (gear)
3. IPv4 → Manual
4. Fill: Address, Netmask, Gateway, DNS
5. Apply

IP Aliases:
├── Add in same Addresses section
├── Netmask required
├── Gateway optional
└── Verify: ip addr show interface

Custom Routes:
├── Routes section in NetworkManager
├── Address: Destination network
├── Netmask: Network mask
├── Gateway: Router IP
└── Verify: route -n

Proxy Configuration:
├── NetworkManager: Network Proxy
├── Firefox: Preferences → Network Settings
├── Default port: 3128
└── Protocols: HTTP, HTTPS, FTP, SOCKS

nmtui (Text Interface):
├── yum install NetworkManager-tui
├── nmtui → Edit/Activate/Hostname
├── Manual IP configuration
└── Custom routes

Verification Commands:
├── ip addr show       → View addresses
├── route -n           → View routes
├── ip route show      → Modern route view
└── ping               → Test connectivity
```
