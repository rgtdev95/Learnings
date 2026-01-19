# Part 4 Quiz: Linux Server Administrator

**Based on:** Linux Bible 10th Edition - Part 4: Becoming a Linux Server Administrator

**Total Questions:** 120

---

## Topic 1: Server Administration Basics

**Q1:** What are the 5 steps of server setup?
<details><summary>Answer</summary>

1. Install
2. Configure
3. Start
4. Secure
5. Monitor

</details>

---

**Q2:** Where are most server configuration files located?
<details><summary>Answer</summary>

`/etc/` directory (or subdirectories like `/etc/httpd/conf/`)

</details>

---

**Q3:** What command enables a service to start at boot in systemd?
<details><summary>Answer</summary>

`systemctl enable servicename`

</details>

---

**Q4:** Why do daemon processes often run as non-root users?
<details><summary>Answer</summary>

To limit damage if the service is compromised - the process only has permissions of that user, not root

</details>

---

## Topic 2: SSH and Remote Access

**Q5:** What command logs in to a remote system via SSH?
<details><summary>Answer</summary>

`ssh username@hostname` or `ssh username@IP_address`

</details>

---

**Q6:** How do you copy a file from local to remote with scp?
<details><summary>Answer</summary>

`scp /local/file username@host:/remote/path`

</details>

---

**Q7:** What's the main advantage of rsync over scp?
<details><summary>Answer</summary>

rsync only copies changed files, preserves permissions/timestamps/symbolic links, and is more efficient for backups

</details>

---

**Q8:** What command generates SSH key pairs?
<details><summary>Answer</summary>

`ssh-keygen`

</details>

---

**Q9:** Where is the client's private SSH key stored?
<details><summary>Answer</summary>

`~/.ssh/id_rsa`

</details>

---

**Q10:** What setting in /etc/ssh/sshd_config disables root login?
<details><summary>Answer</summary>

`PermitRootLogin no`

</details>

---

**Q11:** True or False: sftp uses the FTP protocol.
<details><summary>Answer</summary>

**FALSE** - sftp uses SSH protocol, not FTP. It just has an FTP-like interface.

</details>

---

## Topic 3: System Logging

**Q12:** What daemon provides system logging?
<details><summary>Answer</summary>

`rsyslogd` (rsyslog service)

</details>

---

**Q13:** Where is the rsyslog configuration file?
<details><summary>Answer</summary>

`/etc/rsyslog.conf`

</details>

---

**Q14:** In rsyslog rules, what does `mail.info` mean?
<details><summary>Answer</summary>

Mail facility messages at info level and higher (info, notice, warning, err, crit, alert, emerg)

</details>

---

**Q15:** How do you send logs to a remote loghost in rsyslog?
<details><summary>Answer</summary>

Add `@loghost` before the destination in `/etc/rsyslog.conf`

Example: `*.info @loghost`

</details>

---

**Q16:** What port does rsyslog use for remote logging?
<details><summary>Answer</summary>

**514** (both UDP and TCP)

</details>

---

**Q17:** What command views the systemd journal?
<details><summary>Answer</summary>

`journalctl`

</details>

---

## Topic 4: System Monitoring

**Q18:** What package provides the sar command?
<details><summary>Answer</summary>

`sysstat`

</details>

---

**Q19:** How often does sar collect data by default?
<details><summary>Answer</summary>

Every **10 minutes**

</details>

---

**Q20:** What command shows disk space in human-readable format?
<details><summary>Answer</summary>

`df -h`

</details>

---

**Q21:** What command shows the size of a directory?
<details><summary>Answer</summary>

`du -sh /path/to/directory`

</details>

---

**Q22:** What port does Cockpit use?
<details><summary>Answer</summary>

**9090** (HTTPS)

Access: `https://localhost:9090`

</details>

---

## Topic 5: Enterprise Management

**Q23:** What is PXE booting used for?
<details><summary>Answer</summary>

Automated network-based installation of Linux systems without USB/DVD media

</details>

---

**Q24:** What are the two types of nodes in modern cloud platforms?
<details><summary>Answer</summary>

1. **Management nodes** (control plane/masters) - Orchestrate and manage
2. **Worker nodes** (compute/slaves) - Run actual workloads

</details>

---

**Q25:** True or False: Containers include a full operating system.
<details><summary>Answer</summary>

**FALSE** - Containers share the host OS kernel and only include the application and its dependencies

</details>

---

## Topic 6: Network Basics and DHCP

**Q26:** What are the four essential network components for Internet connectivity?
<details><summary>Answer</summary>

1. Network Interface (hardware)
2. IP Address
3. DNS Server
4. Gateway (default route)

</details>

---

**Q27:** What is the default IPv4 DHCP lease time?
<details><summary>Answer</summary>

**86,400 seconds** (24 hours)

</details>

---

**Q28:** What does a subnet mask of 255.255.255.0 mean in CIDR notation?
<details><summary>Answer</summary>

**/24** - The first 24 bits represent the network portion

</details>

---

## Topic 7: Checking Network Interfaces

**Q29:** What command shows all network interfaces with IP addresses?
<details><summary>Answer</summary>

`ip addr show` or `ifconfig`

</details>

---

**Q30:** How do you test both connectivity and DNS resolution to a host?
<details><summary>Answer</summary>

`ping hostname` (using hostname instead of IP address tests DNS too)

</details>

---

**Q31:** What command traces the route packets take to a destination?
<details><summary>Answer</summary>

`traceroute hostname`

</details>

---

## Topic 8: Configuring Network Interfaces

**Q32:** How do you set a static IP address using NetworkManager GUI?
<details><summary>Answer</summary>

Settings → Network → Interface → Settings (gear) → IPv4 → Change "Automatic (DHCP)" to "Manual" → Fill in Address, Netmask, Gateway, DNS → Apply

</details>

---

**Q33:** True or False: An IP address alias requires a gateway setting.
<details><summary>Answer</summary>

**FALSE** - Aliases only require IP address and netmask; gateway is optional

</details>

---

**Q34:** What is the default proxy server port?
<details><summary>Answer</summary>

**3128**

</details>

---

## Topic 9: Network Configuration Files

**Q35:** Where are network interface configuration files stored in RHEL/Fedora?
<details><summary>Answer</summary>

`/etc/sysconfig/network-scripts/`

Files named: `ifcfg-interface`

</details>

---

**Q36:** What does BOOTPROTO=dhcp mean in an ifcfg file?
<details><summary>Answer</summary>

The interface will obtain its IP address and configuration automatically from a DHCP server

</details>

---

**Q37:** True or False: You should edit /etc/resolv.conf directly.
<details><summary>Answer</summary>

**FALSE** - It's managed by NetworkManager and will be overwritten. Use DNS settings in ifcfg files instead.

</details>

---

**Q38:** What command checks all name resolution sources (hosts file + DNS)?
<details><summary>Answer</summary>

`getent hosts hostname`

</details>

---

## Topic 10: Advanced Networking

**Q39:** What are two benefits of Ethernet channel bonding?
<details><summary>Answer</summary>

1. **High availability** - Failover if NIC or subnet fails
2. **Performance** - Spread traffic across multiple NICs

</details>

---

**Q40:** What kernel parameter must be set to enable Linux as a router?
<details><summary>Answer</summary>

`net.ipv4.ip_forward = 1` (in `/etc/sysctl.conf`)

Or temporarily: `echo 1 > /proc/sys/net/ipv4/ip_forward`

</details>

---

**Q41:** What port does a DHCP server listen on?
<details><summary>Answer</summary>

**UDP port 67**

</details>

---

**Q42:** What bonding mode provides active-backup (failover)?
<details><summary>Answer</summary>

**Mode 1** (active-backup)

</details>

---

## Topic 11: Init Systems and Service Management

**Q43:** What is the PID of the init daemon?
<details><summary>Answer</summary>

**PID 1** (Parent Process ID is 0 - the kernel)

</details>

---

**Q44:** What are the seven SysVinit runlevels and their purposes?
<details><summary>Answer</summary>

- **0** - Halt (shutdown)
- **1** - Single user mode
- **2** - Multiuser (no NFS)
- **3** - Extended multiuser (CLI, network)
- **4** - User defined
- **5** - Graphical mode
- **6** - Reboot

</details>

---

**Q45:** What systemd target is equivalent to runlevel 5?
<details><summary>Answer</summary>

`graphical.target`

</details>

---

**Q46:** What does K15httpd mean in /etc/rc.d/rc5.d/?
<details><summary>Answer</summary>

**K** = Kill (stop) the httpd service
**15** = Kill order (position 15)

</details>

---

**Q47:** What command enables a service at runlevel 3 in SysVinit?
<details><summary>Answer</summary>

`chkconfig --level 3 servicename on`

</details>

---

**Q48:** Where are systemd service unit files stored?
<details><summary>Answer</summary>

- `/lib/systemd/system/` - System defaults
- `/etc/systemd/system/` - Local customizations (takes precedence)

</details>

---

**Q49:** What's the difference between systemctl enable and systemctl start?
<details><summary>Answer</summary>

- **enable** - Service starts at boot (persistent)
- **start** - Service starts now (immediate, temporary)

Often need both!

</details>

---

**Q50:** What does systemctl mask do?
<details><summary>Answer</summary>

Prevents a service from ever running by linking it to `/dev/null`. Even if another service tries to start it, nothing happens.

Undo with: `systemctl unmask service`

</details>

---

**Q51:** True or False: Disabling a service with systemctl stops it immediately.
<details><summary>Answer</summary>

**FALSE** - Disable only prevents it from starting at boot. You must manually stop a running service.

</details>

---

**Q52:** What command reloads systemd after editing unit files?
<details><summary>Answer</summary>

`systemctl daemon-reload`

</details>

---

**Q53:** What's the difference between Wants and Requires in systemd?
<details><summary>Answer</summary>

- **Wants** - Try to start listed units, but if they fail, continue anyway (optional)
- **Requires** - Must start listed units, if they fail, the entire unit fails (mandatory)

</details>

---

**Q54:** How do you set the default target to multi-user.target?
<details><summary>Answer</summary>

`systemctl set-default multi-user.target`

</details>

---

**Q55:** What does the chkconfig line mean in a SysVinit script?
<details><summary>Answer</summary>

Example: `# chkconfig: 2345 25 10`

- **2345** - Runlevels where service starts
- **25** - Start order (S25)
- **10** - Kill order (K10)

</details>

---

**Q56:** Where should custom systemd unit files be stored?
<details><summary>Answer</summary>

`/etc/systemd/system/` - This location takes precedence and won't be overwritten by updates

</details>

---

**Q57:** What command shows if a service is enabled in systemd?
<details><summary>Answer</summary>

`systemctl is-enabled servicename.service`

</details>

---

**🎉 Chapters 13-15 Complete! 57 Questions Total! 🎉**

*Quiz based on Linux Bible 10th Edition - Part 4: Becoming a Linux Server Administrator*



## Topic 12: CUPS Printing

**Q58:** What port does CUPS use by default?
<details><summary>Answer</summary>

**631** (TCP) - Access: `http://localhost:631`

</details>

---

**Q59:** What does PPD stand for?
<details><summary>Answer</summary>

**PostScript Printer Description** - Standardized printer driver format

</details>

---

**Q60:** What command prints a file to the default printer?
<details><summary>Answer</summary>

`lp filename`

</details>

---

**Q61:** How do you view the print queue?
<details><summary>Answer</summary>

`lpq` or `lpstat -o`

</details>

---

**Q62:** What command removes job 133 from the queue?
<details><summary>Answer</summary>

`lprm 133`

</details>

---

**Q63:** True or False: Printer classes can only point to one printer.
<details><summary>Answer</summary>

**FALSE** - Can point to multiple printers for load balancing

</details>

---

**Q64:** What does cupsdisable do?
<details><summary>Answer</summary>

Stops printing but still accepts jobs to queue

</details>

---

## Topic 13: Apache Web Server

**Q65:** What package provides Apache in RHEL/Fedora?
<details><summary>Answer</summary>

`httpd`

</details>

---

**Q66:** What is the default DocumentRoot?
<details><summary>Answer</summary>

`/var/www/html`

</details>

---

**Q67:** What command tests Apache configuration?
<details><summary>Answer</summary>

`apachectl configtest`

</details>

---

**Q68:** What does `Require all granted` do?
<details><summary>Answer</summary>

Allows access to everyone

</details>

---

**Q69:** How do you create an Apache password file?
<details><summary>Answer</summary>

`htpasswd -c /path/.htpasswd username`

</details>

---

**Q70:** What SELinux Boolean allows home directory web content?
<details><summary>Answer</summary>

`httpd_enable_homedirs` - Set with: `setsebool -P httpd_enable_homedirs on`

</details>

---

**Q71:** What command generates an RSA private key?
<details><summary>Answer</summary>

`openssl genrsa -out server.key 2048`

</details>

---

**Q72:** True or False: The first VirtualHost is the default.
<details><summary>Answer</summary>

**TRUE** - Handles requests that don't match any ServerName

</details>

---

## Topic 14: FTP Server (vsftpd)

**Q73:** What does vsftpd stand for?
<details><summary>Answer</summary>

**Very Secure FTP Daemon**

</details>

---

**Q74:** What's the difference between active and passive FTP?
<details><summary>Answer</summary>

- **Active:** Server connects to client (firewall issues)
- **Passive:** Client connects to server (recommended)

</details>

---

**Q75:** What port does FTP use?
<details><summary>Answer</summary>

**21** (TCP) for control

</details>

---

**Q76:** Where is the vsftpd configuration file?
<details><summary>Answer</summary>

`/etc/vsftpd/vsftpd.conf`

</details>

---

**Q77:** What file lists users denied FTP access?
<details><summary>Answer</summary>

`/etc/vsftpd/ftpusers`

</details>

---

**Q78:** What does chroot_local_user=YES do?
<details><summary>Answer</summary>

Restricts users to their home directory

</details>

---

**Q79:** What SELinux Boolean allows FTP home directory access?
<details><summary>Answer</summary>

`ftp_home_dir` - Set with: `setsebool -P ftp_home_dir on`

</details>

---

## Topic 15: Samba File Sharing

**Q80:** What does SMB stand for?
<details><summary>Answer</summary>

**Server Message Block** - Windows file sharing protocol

</details>

---

**Q81:** What are the two main Samba daemons?
<details><summary>Answer</summary>

- **smbd** - File/printer sharing (TCP 445, 139)
- **nmbd** - NetBIOS name service (UDP 137, 138)

</details>

---

**Q82:** Where is the Samba configuration file?
<details><summary>Answer</summary>

`/etc/samba/smb.conf`

</details>

---

**Q83:** What command tests smb.conf syntax?
<details><summary>Answer</summary>

`testparm`

</details>

---

**Q84:** How do you add a Samba user?
<details><summary>Answer</summary>

1. `useradd username`
2. `smbpasswd -a username`

</details>

---

**Q85:** True or False: Samba uses Linux passwords.
<details><summary>Answer</summary>

**FALSE** - Separate password database (smbpasswd)

</details>

---

**Q86:** What does `valid users = @groupname` mean?
<details><summary>Answer</summary>

Only group members can access (@ indicates group)

</details>

---

**Q87:** How do you list Samba shares?
<details><summary>Answer</summary>

`smbclient -L hostname -U username`

</details>

---

**Q88:** What command mounts a Samba share?
<details><summary>Answer</summary>

`mount -t cifs //server/share /mnt -o username=user`

</details>

---

**Q89:** What package is needed to mount CIFS shares?
<details><summary>Answer</summary>

`cifs-utils`

</details>

---

**Q90:** What does the [homes] share do?
<details><summary>Answer</summary>

Auto-creates share for each user's home directory

</details>

---

**🎉 Part 4 Complete! 90 Questions Total! 🎉**

**Coverage:**
- Chapters 13-15: Server Admin, Networking, Services (57 questions)
- Chapters 16-19: Print, Web, FTP, Samba (33 questions)

*Quiz based on Linux Bible 10th Edition - Part 4: Becoming a Linux Server Administrator*

## Topic 16: Troubleshooting Boot Process

**Q91:** What are the 7 steps of the PC boot process?
<details><summary>Answer</summary>

1. Power on
2. BIOS/UEFI firmware
3. Boot loader starts
4. OS selected
5. Kernel and initrd load
6. Init system starts
7. Services start

</details>

---

**Q92:** How do you interrupt the GRUB 2 boot loader?
<details><summary>Answer</summary>

Press any key after BIOS screen

</details>

---

**Q93:** What kernel parameter boots directly to a shell?
<details><summary>Answer</summary>

`init=/bin/bash`

</details>

---

**Q94:** How do you view kernel boot messages?
<details><summary>Answer</summary>

`dmesg` or `journalctl -k`

</details>

---

**Q95:** True or False: UEFI supports disks larger than 2TB.
<details><summary>Answer</summary>

**TRUE** - UEFI supports large disks; BIOS is limited to 2TB

</details>

---

## Topic 17: Software Package Troubleshooting

**Q96:** What command updates all packages in Fedora/RHEL?
<details><summary>Answer</summary>

`dnf update` or `yum update`

</details>

---

**Q97:** How do you rebuild the RPM database?
<details><summary>Answer</summary>

```bash
cd /var/lib/rpm
rm __db*
rpm --initdb
```

</details>

---

**Q98:** What command cleans the DNF cache?
<details><summary>Answer</summary>

`yum clean all` or `dnf clean all`

</details>

---

**Q99:** How do you exclude a package from update?
<details><summary>Answer</summary>

`yum -y --exclude=packagename update`

</details>

---

**Q100:** True or False: yum update upgrades to the next RHEL release.
<details><summary>Answer</summary>

**FALSE** - It updates packages in current release; use `dnf system-upgrade` for new release

</details>

---

## Topic 18: Network Troubleshooting

**Q101:** What command shows network interface status?
<details><summary>Answer</summary>

`ip addr show` or `ifconfig`

</details>

---

**Q102:** How do you test connectivity to a host?
<details><summary>Answer</summary>

`ping -c 2 hostname` or `ping -c 2 IP_address`

</details>

---

**Q103:** What file contains DNS server addresses?
<details><summary>Answer</summary>

`/etc/resolv.conf`

</details>

---

**Q104:** What command scans for open ports?
<details><summary>Answer</summary>

`nmap IP_address`

</details>

---

**Q105:** How do you check which process is listening on port 80?
<details><summary>Answer</summary>

`netstat -tupln | grep :80`

</details>

---

**Q106:** True or False: Pinging 8.8.8.8 tests DNS resolution.
<details><summary>Answer</summary>

**FALSE** - It only tests IP connectivity; use hostname to test DNS

</details>

---

## Topic 19: Memory Troubleshooting

**Q107:** What's the difference between RAM and swap?
<details><summary>Answer</summary>

- **RAM:** Fast, temporary, expensive, near CPU
- **Swap:** Slower, on disk, extends RAM

</details>

---

**Q108:** How do you sort top by memory usage?
<details><summary>Answer</summary>

Press `M` (capital M) in top

</details>

---

**Q109:** What does the RES column in top show?
<details><summary>Answer</summary>

Resident (non-swappable) memory actually being used

</details>

---

**Q110:** How do you drop page caches?
<details><summary>Answer</summary>

`echo 3 > /proc/sys/vm/drop_caches`

</details>

---

**Q111:** What happens when both RAM and swap are full?
<details><summary>Answer</summary>

Out of Memory (OOM) condition - kernel OOM killer starts killing processes

</details>

---

**Q112:** How do you enable Alt+SysRq keys?
<details><summary>Answer</summary>

Add `kernel.sysrq = 1` to `/etc/sysctl.conf`

</details>

---

**Q113:** What does Alt+SysRq+f do?
<details><summary>Answer</summary>

Kills the process with the highest OOM score

</details>

---

## Topic 20: Rescue Mode

**Q114:** What is rescue mode used for?
<details><summary>Answer</summary>

Fixing unbootable Linux systems by booting from rescue media

</details>

---

**Q115:** What does chroot do in rescue mode?
<details><summary>Answer</summary>

Makes the installed system's root directory become the current root (/)

</details>

---

**Q116:** How do you reset root password in rescue mode?
<details><summary>Answer</summary>

```bash
chroot /mnt/sysimage
passwd root
```

</details>

---

**Q117:** What command checks filesystem integrity?
<details><summary>Answer</summary>

`fsck /dev/sda1` (or appropriate device)

</details>

---

**Q118:** True or False: You can run fsck on a mounted filesystem.
<details><summary>Answer</summary>

**FALSE** - Never run fsck on a mounted filesystem; it can cause corruption

</details>

---

**Q119:** How do you remount root filesystem read-write?
<details><summary>Answer</summary>

`mount -o remount,rw /`

</details>

---

**Q120:** What's the first step to enter rescue mode in RHEL?
<details><summary>Answer</summary>

Boot from installation media and select Troubleshooting → Rescue mode

</details>

---

**🎉 Part 4 COMPLETE! 120 Questions Total! 🎉**

**Coverage:**
- Chapters 13-15: Server Admin, Networking, Services (57 questions)
- Chapters 16-19: Print, Web, FTP, Samba (33 questions)
- Chapter 20: Troubleshooting (30 questions)

**All 8 Chapters of Part 4 Covered!**

*Quiz based on Linux Bible 10th Edition - Part 4: Becoming a Linux Server Administrator*
