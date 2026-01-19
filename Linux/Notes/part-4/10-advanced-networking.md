# Advanced Networking

## Ethernet Channel Bonding

**Purpose:** Multiple NICs on same IP address

**Benefits:**
*   **High availability** - Failover if NIC/subnet fails
*   **Performance** - Spread traffic across NICs

### Bonding Modes

| Mode | Name | Description |
|------|------|-------------|
| 0 | balance-rr | Round-robin (load balancing) |
| 1 | active-backup | One active, others standby (failover) |
| 2 | balance-xor | XOR hash distribution |
| 3 | broadcast | Transmit on all interfaces |
| 4 | 802.3ad | IEEE 802.3ad Dynamic link aggregation |
| 5 | balance-tlb | Adaptive transmit load balancing |
| 6 | balance-alb | Adaptive load balancing |

### Configuration Files

**Bond interface:** `/etc/sysconfig/network-scripts/ifcfg-bond0`

```bash
DEVICE=bond0
ONBOOT=yes
IPADDR=192.168.0.50
NETMASK=255.255.255.0
BOOTPROTO=none
BONDING_OPTS="mode=active-backup"
```

**Slave interfaces:** `/etc/sysconfig/network-scripts/ifcfg-eth0`

```bash
DEVICE=eth0
MASTER=bond0
SLAVE=yes
BOOTPROTO=none
ONBOOT=yes
```

**Repeat for eth1, eth2, etc.**

**Load bonding module:** `/etc/modprobe.d/bonding.conf`

```bash
alias bond0 bonding
```

**Activate:**
*   All interfaces set to `ONBOOT=yes`
*   Reboot or restart network

---

## Linux as Router

**Purpose:** Forward packets between network interfaces

**Requirement:** Multiple NICs (2+)

### Enable Packet Forwarding

**Temporary:**
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

**Permanent:** Edit `/etc/sysctl.conf`

```bash
net.ipv4.ip_forward = 1
```

**Apply changes:**
```bash
sysctl -p
```

**Verify:**
```bash
cat /proc/sys/net/ipv4/ip_forward
# Should show: 1
```

### Typical Router Setup

**Scenario:**
*   NIC 1: Connected to Internet (public IP)
*   NIC 2: Connected to private network (192.168.1.0/24)

**Additional configuration:**
*   Firewall with NAT (iptables/firewalld)
*   DHCP server for private network
*   DNS forwarding

---

## DHCP Server

**Package:** `dhcp-server` (RHEL/Fedora)

**Service:** `dhcpd`

**Config:** `/etc/dhcp/dhcpd.conf`

**Port:** UDP 67

### Basic Configuration

**Example `/etc/dhcp/dhcpd.conf`:**

```bash
# Global options
option domain-name "example.com";
option domain-name-servers 8.8.8.8, 8.8.4.4;
default-lease-time 600;
max-lease-time 7200;

# Subnet declaration
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option broadcast-address 192.168.1.255;
}

# Static assignment (by MAC)
host server1 {
  hardware ethernet 00:1B:21:0A:E8:5E;
  fixed-address 192.168.1.50;
}
```

**Start service:**
```bash
systemctl start dhcpd
systemctl enable dhcpd
```

**Firewall:**
```bash
firewall-cmd --permanent --add-service=dhcp
firewall-cmd --reload
```

**Warning:** Don't add DHCP server to network with existing DHCP!

---

## DNS Server (BIND)

**Packages:**
*   `bind` - DNS server
*   `bind-utils` - DNS tools
*   `bind-chroot` - Security (optional)

**Service:** `named`

**Config:** `/etc/named.conf`

**Zone files:** `/var/named/`

**Port:** UDP/TCP 53

### Basic Concepts

**Forward zone:**
*   Hostname → IP address
*   Example: www.example.com → 192.168.1.10

**Reverse zone:**
*   IP address → Hostname
*   Example: 192.168.1.10 → www.example.com

**Zone file:**
*   Contains DNS records
*   A records (IPv4)
*   AAAA records (IPv6)
*   MX records (mail)
*   CNAME records (aliases)

### Security Warning

⚠️ **Public DNS servers must be properly secured!**

**Risks:**
*   Traffic redirection
*   Phishing attacks
*   DNS poisoning

**Best practice:** Start with private/home network testing

---

## Proxy Server (Squid)

**Package:** `squid`

**Service:** `squid`

**Config:** `/etc/squid/squid.conf`

**Port:** 3128 (default)

### Purpose

**Restrict access:**
*   Control which sites users can visit
*   Filter by protocol (HTTP, HTTPS, FTP)
*   Limit by hostname/IP/domain

**Use cases:**
*   School computer labs
*   Corporate networks
*   Parental controls

### Basic Configuration

**Edit `/etc/squid/squid.conf`:**

```bash
# Define allowed networks
acl localnet src 192.168.1.0/24

# Define allowed ports
acl SSL_ports port 443
acl Safe_ports port 80 # HTTP
acl Safe_ports port 443 # HTTPS

# Allow local network
http_access allow localnet

# Deny all other access
http_access deny all

# Squid listening port
http_port 3128
```

**Start service:**
```bash
systemctl start squid
systemctl enable squid
```

**Client configuration:**
*   Set proxy in browser
*   Or set in NetworkManager

**Blacklists:**
*   Available for blocking inappropriate sites
*   Can block by category

---

## Network Address Translation (NAT)

**Purpose:** Allow private IPs to access Internet

**How it works:**
1. Private network uses 192.168.x.x addresses
2. Router translates to public IP
3. Internet sees only public IP
4. Router tracks connections
5. Returns traffic to correct private IP

**Configuration:** Via iptables/firewalld (Chapter 25)

---

## Review Questions

1. What are two benefits of Ethernet channel bonding?

2. What kernel parameter enables packet forwarding?

3. What port does DHCP server use?

4. What service provides DNS in Linux?

5. True or False: You should add a DHCP server to any network.

6. What is the default Squid proxy port?

7. What bonding mode provides failover?

8. Where are BIND zone files stored?

---

## Quick Reference

```
Ethernet Bonding:
Files:
├── ifcfg-bond0 → Bond interface (IP address)
├── ifcfg-eth0  → Slave (MASTER=bond0, SLAVE=yes)
└── /etc/modprobe.d/bonding.conf → alias bond0 bonding

Modes:
├── 0 → balance-rr (load balancing)
├── 1 → active-backup (failover)
└── 4 → 802.3ad (LACP)

Packet Forwarding (Router):
├── Temporary: echo 1 > /proc/sys/net/ipv4/ip_forward
├── Permanent: /etc/sysctl.conf
│   └── net.ipv4.ip_forward = 1
└── Verify: cat /proc/sys/net/ipv4/ip_forward

DHCP Server:
├── Package: dhcp-server
├── Service: dhcpd
├── Config: /etc/dhcp/dhcpd.conf
├── Port: UDP 67
└── Firewall: firewall-cmd --add-service=dhcp

DNS Server (BIND):
├── Package: bind
├── Service: named
├── Config: /etc/named.conf
├── Zones: /var/named/
└── Port: UDP/TCP 53

Proxy Server (Squid):
├── Package: squid
├── Service: squid
├── Config: /etc/squid/squid.conf
├── Port: 3128
└── Features: Filter sites, protocols, domains

Network Services:
├── Router    → Forwards packets between networks
├── DHCP      → Assigns IP addresses
├── DNS       → Resolves hostnames
├── Proxy     → Filters/controls Internet access
└── NAT       → Translates private to public IPs
```
