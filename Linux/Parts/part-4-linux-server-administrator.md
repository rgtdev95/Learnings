# Part 4: Becoming a Linux Server Administrator

**Source:** Linux Bible - 10th Edition, Part IV

**Overview:** This part focuses on administering Linux servers, covering remote access, networking, services, and various server types (print, web, FTP, file sharing, NFS).

---

## 📚 Chapter Coverage

### Chapter 13: Understanding Server Administration

| # | Topic | Notes File |
|---|-------|------------|
| 01 | Server Admin Basics | [01-server-admin-basics.md](../Notes/part-4/01-server-admin-basics.md) |
| 02 | SSH and Remote Access | [02-ssh-remote-access.md](../Notes/part-4/02-ssh-remote-access.md) |
| 03 | System Logging | [03-system-logging.md](../Notes/part-4/03-system-logging.md) |
| 04 | System Monitoring | [04-system-monitoring.md](../Notes/part-4/04-system-monitoring.md) |
| 05 | Enterprise Server Management | [05-enterprise-server-management.md](../Notes/part-4/05-enterprise-server-management.md) |

### Chapter 14: Administering Networking

| # | Topic | Notes File |
|---|-------|------------|
| 06 | Network Basics and DHCP | [06-network-basics-dhcp.md](../Notes/part-4/06-network-basics-dhcp.md) |
| 07 | Checking Network Interfaces | [07-checking-network-interfaces.md](../Notes/part-4/07-checking-network-interfaces.md) |
| 08 | Configuring Network Interfaces | [08-configuring-network-interfaces.md](../Notes/part-4/08-configuring-network-interfaces.md) |
| 09 | Network Configuration Files | [09-network-config-files.md](../Notes/part-4/09-network-config-files.md) |
| 10 | Advanced Networking | [10-advanced-networking.md](../Notes/part-4/10-advanced-networking.md) |

### Chapter 15: Starting and Stopping Services

| # | Topic | Notes File |
|---|-------|------------|
| 11 | Init Systems Overview | [11-init-systems-overview.md](../Notes/part-4/11-init-systems-overview.md) |
| 12 | SysVinit Service Management | [12-sysvinit-service-management.md](../Notes/part-4/12-sysvinit-service-management.md) |
| 13 | systemd Service Management | [13-systemd-service-management.md](../Notes/part-4/13-systemd-service-management.md) |
| 14 | Managing Services | [14-managing-services.md](../Notes/part-4/14-managing-services.md) |
| 15 | Adding Custom Services | [15-adding-custom-services.md](../Notes/part-4/15-adding-custom-services.md) |

### Chapter 16: Configuring a Print Server

| # | Topic | Notes File |
|---|-------|------------|
| 16 | CUPS Printing Basics | [16-cups-printing-basics.md](../Notes/part-4/16-cups-printing-basics.md) |
| 17 | Setting Up Printers | [17-setting-up-printers.md](../Notes/part-4/17-setting-up-printers.md) |
| 18 | Printing Commands | [18-printing-commands.md](../Notes/part-4/18-printing-commands.md) |
| 19 | Configuring Print Servers | [19-configuring-print-servers.md](../Notes/part-4/19-configuring-print-servers.md) |

### Chapter 17: Configuring a Web Server

| # | Topic | Notes File |
|---|-------|------------|
| 20 | Apache Basics | [20-apache-basics.md](../Notes/part-4/20-apache-basics.md) |
| 21 | Apache Configuration | [21-apache-configuration.md](../Notes/part-4/21-apache-configuration.md) |
| 22 | Apache Security | [22-apache-security.md](../Notes/part-4/22-apache-security.md) |
| 23 | Apache SSL Certificates | [23-apache-ssl-certificates.md](../Notes/part-4/23-apache-ssl-certificates.md) |
| 24 | Apache Troubleshooting | [24-apache-troubleshooting.md](../Notes/part-4/24-apache-troubleshooting.md) |

### Chapter 18: Configuring an FTP Server

| # | Topic | Notes File |
|---|-------|------------|
| 25 | FTP Basics | [25-ftp-basics.md](../Notes/part-4/25-ftp-basics.md) |
| 26 | vsftpd Installation | [26-vsftpd-installation.md](../Notes/part-4/26-vsftpd-installation.md) |
| 27 | vsftpd Security | [27-vsftpd-security.md](../Notes/part-4/27-vsftpd-security.md) |
| 28 | vsftpd Configuration | [28-vsftpd-configuration.md](../Notes/part-4/28-vsftpd-configuration.md) |
| 29 | FTP Clients | [29-ftp-clients.md](../Notes/part-4/29-ftp-clients.md) |

### Chapter 19: Configuring a Windows File Sharing (Samba) Server

| # | Topic | Notes File |
|---|-------|------------|
| 30 | Samba Basics | [30-samba-basics.md](../Notes/part-4/30-samba-basics.md) |
| 31 | Samba Installation | [31-samba-installation.md](../Notes/part-4/31-samba-installation.md) |
| 32 | Samba Security | [32-samba-security.md](../Notes/part-4/32-samba-security.md) |
| 33 | Samba File Sharing | [33-samba-file-sharing.md](../Notes/part-4/33-samba-file-sharing.md) |
| 34 | Samba Clients | [34-samba-clients.md](../Notes/part-4/34-samba-clients.md) |


---

## 🎯 Learning Objectives

By the end of this part, you should be able to:
- **Understand Server Setup:** Follow the 5-step process (install, configure, start, secure, monitor) for any Linux server.
- **Use SSH Tools:** Connect remotely with ssh, transfer files with scp/rsync/sftp, set up key-based authentication.
- **Configure Logging:** Set up rsyslog for local and remote logging, use logwatch for daily reports.
- **Monitor Systems:** Use sar for performance monitoring, check disk space with df/du, watch activity with Cockpit.
- **Understand Enterprise Concepts:** Know about PXE booting, containers, cloud platforms, and infrastructure as code.
- **Configure Networking:** Set up network interfaces (DHCP/static), check connectivity, configure routes and proxies.
- **Manage Network Files:** Edit ifcfg files, understand /etc/hosts, /etc/resolv.conf, use nmtui for text-based config.
- **Advanced Networking:** Set up bonding, enable routing, configure DHCP/DNS/proxy servers.
- **Understand Init Systems:** Know the difference between SysVinit and systemd, runlevels vs targets.
- **Manage Services:** Start/stop/restart/reload services, enable/disable at boot, check status.
- **Configure Runlevels/Targets:** Set default runlevel or target, change between them.
- **Add Custom Services:** Create service scripts/units, add to init system, troubleshoot issues.
- **Configure Print Servers:** Set up CUPS, add printers (local/remote), manage print queues, share printers.
- **Manage Web Servers:** Install Apache, configure virtual hosts, set up SSL/TLS, troubleshoot issues.
- **Deploy FTP Servers:** Install vsftpd, configure security (firewall/SELinux), manage users, set up passive mode.
- **Set Up File Sharing:** Install Samba, create shares, manage permissions, connect from Windows/Linux clients.


---

## ⌨️ Key Commands

```bash
# SSH Remote Access
ssh user@host                    # Remote login
ssh user@host command            # Remote execution
ssh -X user@host                 # X11 forwarding
scp file user@host:/path         # Secure copy
rsync -avl src dest              # Sync files
sftp user@host                   # Interactive file transfer
ssh-keygen                       # Generate SSH keys
ssh-copy-id user@host            # Copy public key

# System Logging
systemctl restart rsyslog        # Restart logging service
journalctl                       # View systemd journal
journalctl -f                    # Follow journal
journalctl -u service            # Service-specific logs
mail                             # Read logwatch emails

# System Monitoring
sar -u                           # CPU usage
sar -d                           # Disk activity
sar -n DEV                       # Network activity
df -h                            # Disk space (human-readable)
du -sh /path                     # Directory size
find / -size +100M               # Find large files

# Network Configuration
ip addr show                     # View network interfaces
ip route show                    # View routes
ping host                        # Test connectivity
route -n                         # View routing table
traceroute host                  # Trace route to host
nmtui                            # Text-based network config
ifconfig                         # Legacy interface info
hostname                         # Show hostname
hostnamectl set-hostname name    # Set hostname

# Service Management (SysVinit)
service svc status/start/stop    # Manage service
chkconfig --list                 # List services
chkconfig --level 3 svc on       # Enable at runlevel
runlevel                         # Show runlevel
init 3                           # Change runlevel

# Service Management (systemd)
systemctl status svc.service     # Check status
systemctl start/stop svc         # Start/stop service
systemctl restart/reload svc     # Restart/reload
systemctl enable/disable svc     # Enable/disable at boot
systemctl mask/unmask svc        # Mask/unmask service
systemctl list-unit-files        # List all units
systemctl get-default            # Show default target
systemctl set-default target     # Set default target
systemctl isolate target         # Change to target
systemctl daemon-reload          # Reload unit files



# Service Management
systemctl status service         # Check status
systemctl start service          # Start service
systemctl enable service         # Enable at boot
systemctl restart service        # Restart service

# CUPS Printing
lp file.txt                      # Print file
lp -d printer file.txt           # Print to specific printer
lpstat -t                        # Show all status
lpq                              # View print queue
lprm job-id                      # Remove print job
system-config-printer            # GUI printer config

# Apache Web Server
systemctl start httpd            # Start Apache
apachectl configtest             # Test configuration
apachectl graceful               # Graceful restart
htpasswd -c /path/.htpasswd user # Create password file
openssl genrsa -out key.key 2048 # Generate private key

# FTP Server (vsftpd)
systemctl start vsftpd           # Start vsftpd
ftp localhost                    # Test connection
lftp hostname                    # Advanced FTP client
smbpasswd -a user                # Add FTP user

# Samba File Sharing
systemctl start smb nmb          # Start Samba services
testparm                         # Test smb.conf syntax
smbpasswd -a user                # Add Samba user
smbclient -L host -U user        # List shares
smbclient //server/share -U user # Connect to share
mount -t cifs //server/share /mnt -o username=user  # Mount share
pdbedit -L                       # List Samba users
```

---

## 📝 Quiz

Practice your knowledge: [Part 4 Quiz](../Quiz/Part-4/part-4-quiz.md)

---

## 🔗 Related Parts

- [Part 3: Becoming a Linux System Administrator](part-3-linux-system-administrator.md) - Foundation for server administration
- Part 5: Learning Linux Security Techniques - Securing servers (coming soon)
- Part 6: Engaging with Cloud Computing - Advanced cloud platforms (coming soon)

---

**Next:** Chapter 14 - Administering Networking (coming soon)

### Chapter 20: Troubleshooting Linux

| # | Topic | Notes File |
|---|-------|------------|
| 35 | Boot Troubleshooting | [35-boot-troubleshooting.md](../Notes/part-4/35-boot-troubleshooting.md) |
| 36 | Software Package Troubleshooting | [36-software-package-troubleshooting.md](../Notes/part-4/36-software-package-troubleshooting.md) |
| 37 | Network Troubleshooting | [37-network-troubleshooting.md](../Notes/part-4/37-network-troubleshooting.md) |
| 38 | Memory Troubleshooting | [38-memory-troubleshooting.md](../Notes/part-4/38-memory-troubleshooting.md) |
| 39 | Rescue Mode | [39-rescue-mode.md](../Notes/part-4/39-rescue-mode.md) |

