# Checking Network Interfaces

## NetworkManager GUI

**Access:** Click network icon in top-right corner of desktop

**View connection details:**
1. Click network icon
2. Select active connection
3. View IPv4/IPv6 addresses, DNS, gateway

**Security settings:**
*   Click "Settings" → "Wi-Fi" or "Network"
*   Select connection → Settings (gear icon)
*   View/change security type and password

---

## Cockpit Web Interface

**Access:** `https://localhost:9090/network`

**Features:**
*   View all network interfaces at once
*   Real-time traffic graphs (send/receive)
*   Firewall configuration
*   Add bonds, teams, bridges, VLANs

**Firewall view:**
*   Shows open services (DHCPv6, mDNS, SSH, Samba)
*   UDP and TCP ports listed separately
*   Easy service enable/disable

---

## Command-Line Tools

### ip addr show

**View all interfaces:**
```bash
ip addr show
```

**Output shows:**
*   Interface name (lo, enp4s0, wlp2s0)
*   MAC address
*   IPv4 and IPv6 addresses
*   Broadcast address
*   Interface state (UP/DOWN)

**View specific interface:**
```bash
ip addr show wlp2s0
```

**With statistics:**
```bash
ip -s addr show
```

**Example output:**
```
3: wlp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether e0:06:e6:83:ac:c7
    inet 192.168.1.83/24 brd 192.168.1.255
    inet6 fe80::25ff:8129:751b:23e3/64
```

---

### ifconfig

**View all interfaces:**
```bash
ifconfig
```

**View specific interface:**
```bash
ifconfig wlp2s0
```

**Shows:**
*   IP addresses (IPv4/IPv6)
*   Netmask and broadcast
*   MAC address (ether)
*   RX/TX packets and bytes
*   Errors and dropped packets

**Example:**
```
wlp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>
        inet 192.168.1.83  netmask 255.255.255.0
        ether e0:06:e6:83:ac:c7
        RX packets 208402  bytes 250962570 (239.3 MiB)
        TX packets 113589  bytes 13240384 (12.6 MiB)
```

---

### ping - Check Connectivity

**Test connection to host:**
```bash
ping google.com
ping 192.168.1.1
```

**Output:**
```
64 bytes from host1 (192.168.1.15): icmp_seq=1 ttl=64 time=0.062 ms
```

**Stop with:** Ctrl+C

**Limited pings:**
```bash
ping -c 4 google.com  # Send 4 pings only
```

**Why use hostname?**
*   Tests connectivity
*   Tests DNS resolution
*   Confirms full network stack

---

### route - View Routing Table

**Show routes:**
```bash
route
route -n  # Numeric (no DNS lookup)
```

**Example output:**
```
Destination     Gateway         Genmask         Iface
default         192.168.1.1     0.0.0.0         wlp2s0
192.168.1.0     0.0.0.0         255.255.255.0   wlp2s0
```

**Flags:**
*   U = Route is up
*   G = Gateway
*   H = Host route

---

### ip route show

**Modern alternative to route:**
```bash
ip route show
```

**Example:**
```
default via 192.168.122.1 dev ens3 proto dhcp
192.168.122.0/24 dev ens3 proto kernel scope link
```

---

### traceroute - Trace Route

**Follow path to destination:**
```bash
traceroute google.com
```

**Shows:**
*   Each hop along the route
*   IP address of each router
*   Time to reach each hop

**Example:**
```
1  192.168.1.1 (192.168.1.1)  1.234 ms
2  10.0.0.1 (10.0.0.1)  5.678 ms
3  74.125.235.136 (74.125.235.136)  15.234 ms
```

**Use case:** Find network bottlenecks

---

### hostname - View Hostname

**Show full hostname:**
```bash
hostname
```

**Show domain only:**
```bash
dnsdomainname
```

**Example:**
```
$ hostname
spike.example.com

$ dnsdomainname
example.com
```

---

## Network Interface Naming

### Modern Naming (Predictable)

**Format:** Based on physical location

**Examples:**
*   `enp4s0` - Ethernet, PCI bus 4, slot 0
*   `wlp2s0` - Wireless, PCI bus 2, slot 0
*   `em1` - Embedded Ethernet port 1
*   `p3p1` - PCI bus 3, port 1

### Old Naming (Generic)

**Examples:**
*   `eth0` - First Ethernet
*   `eth1` - Second Ethernet
*   `wlan0` - First wireless

**Why change?**
*   Consistent across reboots
*   Reflects physical location
*   Easier troubleshooting

---

## Review Questions

1. What command shows all network interfaces with IP addresses?

2. What port does Cockpit use?

3. How do you test connectivity to a remote host?

4. What command shows the routing table?

5. True or False: ping tests both connectivity and DNS resolution.

6. What does traceroute show?

7. How do you view statistics with ip command?

8. What does the interface name "enp4s0" mean?

---

## Quick Reference

```
GUI Tools:
├── NetworkManager → Desktop network icon
└── Cockpit        → https://localhost:9090/network

View Interfaces:
├── ip addr show           → All interfaces
├── ip addr show wlp2s0    → Specific interface
├── ip -s addr show        → With statistics
└── ifconfig               → Legacy tool

Test Connectivity:
├── ping google.com        → Test connection + DNS
├── ping -c 4 host         → Limited pings
└── ping 192.168.1.1       → Test by IP

View Routes:
├── ip route show          → Modern
├── route                  → Legacy
└── route -n               → Numeric (faster)

Trace Route:
└── traceroute google.com  → Follow path

Hostname:
├── hostname               → Full hostname
└── dnsdomainname          → Domain only

Interface Names:
├── enp4s0 → Ethernet, PCI bus 4, slot 0
├── wlp2s0 → Wireless, PCI bus 2, slot 0
└── em1    → Embedded port 1
```
