# Network Troubleshooting

## Outgoing Connection Troubleshooting

**Problem:** Cannot reach websites or remote hosts

**Troubleshooting steps:**
1. Check network interfaces
2. Check physical connections
3. Check routes
4. Check hostname resolution

---

## View Network Interfaces

**Check interface status:**
```bash
ip addr show
```

**Example output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436
    inet 127.0.0.1/8 scope host lo
2: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 state DOWN
    link/ether f0:de:f1:28:46:d9
```

**Interface states:**
*   `UP` - Interface is up
*   `DOWN` - Interface is down
*   `NO-CARRIER` - No physical connection

**Modern naming (RHEL 8+, Fedora):**
*   `enp11s0` - Ethernet (en), PCI 11 (p11), slot 0 (s0)
*   `wlp3s0` - Wireless (wl), PCI 3 (p3), slot 0 (s0)

---

## Check Physical Connections

**Wired connections:**
*   Verify cable is plugged in
*   Check correct NIC port
*   Ensure NIC is seated in slot
*   Check if disabled in BIOS

**Identify NIC:**
```bash
ethtool -p eth0  # Blinks NIC LED
```
(Press Ctrl+C to stop blinking)

**Wireless connections:**
*   Check for physical disable switch
*   Verify not disabled in BIOS
*   Check NetworkManager for wireless interface

---

## Check Routes

**View routing table:**
```bash
ip route show
```

**Example output:**
```
default via 192.168.122.1 dev ens3
192.168.122.0/24 dev ens3 scope link src 192.168.122.194
```

**Test default gateway:**
```bash
ping -c 2 192.168.122.1
```

**Test external connectivity:**
```bash
ping -c 2 8.8.8.8  # Google DNS
```

**Trace route to host:**
```bash
traceroute www.google.com
```

**Monitor route performance:**
```bash
mtr www.google.com  # Continuous monitoring
```

---

## Check Hostname Resolution

**DNS server location:**
*   `/etc/resolv.conf`

**Example resolv.conf:**
```
search example.com
nameserver 192.168.0.254
nameserver 192.168.0.253
```

**Test DNS server reachability:**
```bash
ping -c 2 192.168.0.254
```

**Test DNS resolution:**
```bash
host www.google.com 192.168.0.254
dig @192.168.0.254 www.google.com
```

**Example host output:**
```
www.google.com has address 172.217.13.228
www.google.com has IPv6 address 2607:f8b0:4004:809::2004
```

**Fix DNS servers:**

**With NetworkManager:**
1. Add `PEERDNS=no` to `/etc/sysconfig/network-scripts/ifcfg-eth0`
2. Set `DNS1=192.168.0.254`
3. Restart networking

**With network service:**
1. Set `PEERDNS=no` in ifcfg file
2. Edit `/etc/resolv.conf` directly

---

## Incoming Connection Troubleshooting

**Problem:** Clients cannot reach your server

**Troubleshooting steps:**
1. Check if client can reach system
2. Check if service is available
3. Check firewall
4. Check service configuration

---

## Check Service Availability

**Scan for open ports:**
```bash
nmap 192.168.0.119
```

**Example output:**
```
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https
631/tcp open  ipp
```

**Port states:**
*   `open` - Service listening
*   `closed` - Port accessible, no service
*   `filtered` - Firewall blocking

---

## Check Firewall

**View firewall rules (iptables):**
```bash
iptables -vnL
```

**Check for HTTP/HTTPS rules:**
```
0 0 ACCEPT tcp -- * * 0.0.0.0/0 0.0.0.0/0 state NEW tcp dpt:80
0 0 ACCEPT tcp -- * * 0.0.0.0/0 0.0.0.0/0 state NEW tcp dpt:443
```

**With firewalld (RHEL 8+, Fedora):**
```bash
firewall-cmd --list-services
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
```

**Add iptables rules:**

Edit `/etc/sysconfig/iptables`:
```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```

**Restart iptables:**
```bash
systemctl restart iptables.service
```

**Firewall troubleshooting:**
*   Check rule order (ACCEPT before DROP)
*   Look for denied hosts (-s IP -j DROP)
*   Verify port numbers match service

---

## Check Service Status

**Check if service is running:**
```bash
systemctl status httpd.service
```

**Check listening ports:**
```bash
netstat -tupln | grep httpd
```

**Example output:**
```
tcp 0 0 :::80   :::* LISTEN 2567/httpd
tcp 0 0 :::443  :::* LISTEN 2567/httpd
```

**Listening on specific interface:**
```
tcp 0 0 127.0.0.1:80 :::* LISTEN 2567/httpd
```

**Configure listening address:**

Edit `/etc/httpd/conf/httpd.conf`:
```
Listen 80                    # All interfaces
Listen 192.168.0.100:80      # Specific interface
```

---

## Review Questions

1. What command shows network interface status?

2. How do you test connectivity to the default gateway?

3. Where are DNS servers configured?

4. What command tests DNS resolution?

5. What tool scans for open ports?

6. How do you view firewall rules with iptables?

7. What command shows which process is listening on a port?

8. True or False: `ping 8.8.8.8` tests DNS resolution.

---

## Quick Reference

```
View Interfaces:
└── ip addr show

Test Connectivity:
├── ping -c 2 IP_ADDRESS
├── ping -c 2 8.8.8.8        → Test external
└── traceroute hostname      → Trace route

View Routes:
├── ip route show
└── route -n

DNS Troubleshooting:
├── Config: /etc/resolv.conf
├── Test: host hostname DNS_IP
├── Test: dig @DNS_IP hostname
└── Ping: ping -c 2 DNS_IP

Port Scanning:
└── nmap IP_ADDRESS

Firewall (firewalld):
├── firewall-cmd --list-services
├── firewall-cmd --permanent --add-service=http
└── firewall-cmd --reload

Firewall (iptables):
├── iptables -vnL
├── Edit: /etc/sysconfig/iptables
└── Restart: systemctl restart iptables

Check Service:
├── systemctl status service
├── netstat -tupln | grep service
└── ps -ef | grep service

Network Tools:
├── ip          → Interface/route management
├── ping        → Test connectivity
├── traceroute  → Trace route
├── mtr         → Monitor route
├── host        → DNS lookup
├── dig         → DNS query
├── nmap        → Port scanner
├── netstat     → Network statistics
└── ethtool     → Ethernet tool
```
