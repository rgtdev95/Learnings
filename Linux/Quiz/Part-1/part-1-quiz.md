# Part 1: Getting Started - Quiz & Flashcards

> **How to use:** Click on "Answer" to reveal the answer. Test yourself before revealing!

---

## Topic 1: Understanding What Linux Is

**Q1:** What is Linux?
<details>
<summary>Answer</summary>

Linux is a free, open-source operating system **kernel** created by Linus Torvalds in 1991. Combined with GNU tools and other software, it forms a complete operating system often called GNU/Linux.

</details>

---

**Q2:** What is the difference between the Linux kernel and a Linux distribution?
<details>
<summary>Answer</summary>

- **Kernel:** The core component that manages hardware, processes, and memory
- **Distribution:** A complete OS package that includes the kernel + GNU tools + package manager + desktop environment + applications

Example: Ubuntu is a distribution that uses the Linux kernel.

</details>

---

**Q3:** Name 5 core components of a Linux system.
<details>
<summary>Answer</summary>

1. Hardware detection and management
2. Process management
3. Memory management
4. File system control
5. User interface (CLI/GUI)
6. User authentication
7. Administrative utilities
8. Service management
9. Programming tools

(Any 5 of these are correct)

</details>

---

**Q4:** What command displays the kernel version?
<details>
<summary>Answer</summary>

```bash
uname -r
```

For complete system information: `uname -a`

</details>

---

**Q5:** What does "open source" mean?
<details>
<summary>Answer</summary>

Open source means the source code is freely available for anyone to:
- **View** - See how it works
- **Modify** - Change it for your needs
- **Distribute** - Share it with others

It ensures transparency and community collaboration.

</details>

---

## Topic 2: How Linux Differs from Other Operating Systems

**Q6:** Name 5 ways Linux differs from Windows.
<details>
<summary>Answer</summary>

1. **Open Source** - Linux source code is available; Windows is proprietary
2. **Free** - Linux is free; Windows requires a paid license
3. **Customizable** - Linux allows changing any component; Windows is limited
4. **Security Model** - Linux uses permissions + sudo; Windows uses ACL
5. **Package Management** - Linux has centralized repos; Windows uses scattered downloads

</details>

---

**Q7:** What are the three main desktop environments in Linux?
<details>
<summary>Answer</summary>

1. **GNOME** - Default on Ubuntu, Fedora
2. **KDE Plasma** - Feature-rich, customizable
3. **XFCE** - Lightweight, runs on older hardware

Others include: Cinnamon, MATE, LXQt, i3

</details>

---

**Q8:** What is `sudo` and why is it a security feature?
<details>
<summary>Answer</summary>

**sudo** = "superuser do"

It allows a normal user to run commands with administrator (root) privileges temporarily.

**Security benefit:** Users work with limited permissions by default. They only escalate privileges when needed, reducing the risk of accidental or malicious system damage.

```bash
sudo apt update   # Runs with root privileges
```

</details>

---

**Q9:** What is the Linux file permission format?
<details>
<summary>Answer</summary>

```
rwxrwxrwx
│││ │││ │││
│││ │││ └── Others permissions (read, write, execute)
│││ └── Group permissions
└── Owner permissions
```

Example: `chmod 755 file.sh` = rwxr-xr-x (owner: full, group/others: read+execute)

</details>

---

## Topic 3: Linux History

**Q10:** When and by whom was Unix created?
<details>
<summary>Answer</summary>

**Year:** 1969
**Where:** Bell Labs (AT&T)
**By:** Ken Thompson and Dennis Ritchie

Unix is the ancestor of Linux and many other operating systems.

</details>

---

**Q11:** Who created Linux and when?
<details>
<summary>Answer</summary>

**Who:** Linus Torvalds (Finnish computer science student)
**When:** 1991
**Originally:** A personal project to replace MINIX

He announced it on Usenet with the famous "just a hobby, won't be big and professional" message.

</details>

---

**Q12:** What is the GNU Project and who started it?
<details>
<summary>Answer</summary>

**Started:** 1983
**By:** Richard Stallman
**Purpose:** Create a complete free (libre) operating system

**GNU = GNU's Not Unix** (recursive acronym)

GNU provided essential tools (gcc, bash, coreutils) but lacked a kernel. Linux filled that gap.

</details>

---

**Q13:** What is the GPL license?
<details>
<summary>Answer</summary>

**GPL = GNU General Public License**

Key requirements:
- Freedom to use, modify, and distribute
- Source code must remain available
- Modifications must also be GPL-licensed ("copyleft")

Linux kernel is released under GPL v2.

</details>

---

**Q14:** What is the relationship between Unix, GNU, and Linux?
<details>
<summary>Answer</summary>

```
Unix (1969) ─┐
             │──▶ Inspiration for
GNU (1983) ──┴───▶ Linux (1991)

GNU + Linux Kernel = Complete OS
```

- Unix provided the design philosophy
- GNU provided the tools
- Linux provided the kernel

</details>

---

## Topic 4: How Linux Distributions Emerged

**Q15:** What is a Linux distribution?
<details>
<summary>Answer</summary>

A distribution (distro) is a complete, ready-to-use Linux operating system that includes:
- Linux kernel
- GNU tools and utilities
- Package manager
- Desktop environment (optional)
- Pre-installed applications
- Configuration and branding

Examples: Ubuntu, Fedora, Arch Linux

</details>

---

**Q16:** Name the three major distribution families and their package managers.
<details>
<summary>Answer</summary>

| Family | Package Format | Manager | Example Distros |
|--------|---------------|---------|-----------------|
| **Debian** | .deb | apt | Ubuntu, Mint, Debian |
| **Red Hat** | .rpm | dnf | Fedora, RHEL, Rocky |
| **Arch** | .pkg.tar.zst | pacman | Arch, Manjaro |

</details>

---

**Q17:** What is the difference between a fixed release and rolling release?
<details>
<summary>Answer</summary>

**Fixed Release (e.g., Ubuntu):**
- Major updates every 6 months or 2 years (LTS)
- Stable, tested packages
- Good for servers and production

**Rolling Release (e.g., Arch):**
- Continuous updates, no version numbers
- Always latest software
- Good for desktops wanting cutting-edge features

</details>

---

**Q18:** What does LTS stand for and why is it important?
<details>
<summary>Answer</summary>

**LTS = Long Term Support**

- Extended support period (typically 5 years)
- Security updates and bug fixes guaranteed
- No major feature changes (stability)
- Recommended for servers and production environments

Example: Ubuntu 22.04 LTS is supported until 2027.

</details>

---

**Q19:** Which command shows your distribution information?
<details>
<summary>Answer</summary>

```bash
cat /etc/os-release
# or
lsb_release -a
```

These commands show the distribution name, version, and ID.

</details>

---

## Topic 5: How Companies Make Money with Linux

**Q20:** How can companies profit from free software?
<details>
<summary>Answer</summary>

The **software** is free, but companies sell:
- Enterprise support and subscriptions
- Consulting and implementation services
- Training and certifications
- Cloud infrastructure running Linux
- Hardware pre-installed with Linux
- Premium/enterprise features (dual licensing)

"Free as in speech, not always free as in beer."

</details>

---

**Q21:** What does Red Hat's enterprise subscription include?
<details>
<summary>Answer</summary>

- Certified, tested software packages
- 10-year support lifecycle
- Security patches and updates
- 24/7 technical support
- Legal indemnification (protection from lawsuits)
- SLA guarantees

Red Hat generates over $5 billion annually with this model.

</details>

---

**Q22:** Why do major cloud providers prefer Linux?
<details>
<summary>Answer</summary>

- **No licensing costs** - Can run unlimited instances without per-core fees
- **Customizable** - Can optimize for their hardware
- **Reliable** - Proven in large-scale deployments
- **Community support** - Large talent pool
- **Containerization** - Built for Docker/Kubernetes

~90% of public cloud workloads run Linux.

</details>

---

**Q23:** What is the "open core" or "dual licensing" model?
<details>
<summary>Answer</summary>

**Open Core Model:**
```
Community Edition (Free) ──▶ Open source, basic features
Enterprise Edition (Paid) ─▶ Adds premium features + support
```

Examples:
- GitLab CE vs EE
- MySQL Community vs Enterprise
- MongoDB Community vs Enterprise

</details>

---

**Q24:** Name 3 Linux certification programs.
<details>
<summary>Answer</summary>

1. **Red Hat** - RHCSA, RHCE, RHCA
2. **Linux Foundation** - LFCS, LFCE, CKA (Kubernetes)
3. **CompTIA** - Linux+
4. **LPI** - LPIC-1, LPIC-2, LPIC-3

These certifications validate Linux skills for employment.

</details>

---

## Quick Command Reference Quiz

**Q25:** Match the command to its purpose:

| Command | Purpose |
|---------|---------|
| `uname -r` | ? |
| `free -h` | ? |
| `df -h` | ? |
| `cat /etc/os-release` | ? |
| `chmod 755 file` | ? |

<details>
<summary>Answer</summary>

| Command | Purpose |
|---------|---------|
| `uname -r` | Show kernel version |
| `free -h` | Show memory usage |
| `df -h` | Show disk usage |
| `cat /etc/os-release` | Show distribution info |
| `chmod 755 file` | Set file permissions (rwxr-xr-x) |

</details>

---

## Bonus: Rapid Fire True/False

**Q26-30:** True or False?
<details>
<summary>Questions</summary>

26. Linux was created by Richard Stallman. 
27. Ubuntu is based on Debian. 
28. The Linux kernel is licensed under GPL. 
29. Windows uses the .deb package format. 
30. "sudo" gives permanent root access.

</details>

<details>
<summary>Answers</summary>

26. **FALSE** - Linux was created by Linus Torvalds. Stallman started GNU.
27. **TRUE** - Ubuntu is a Debian derivative.
28. **TRUE** - Linux uses GPL version 2.
29. **FALSE** - Windows uses .msi/.exe. Debian uses .deb.
30. **FALSE** - sudo gives temporary root access for one command.

</details>

---

## Study Tips

1. **Active recall:** Try to answer before revealing
2. **Spaced repetition:** Review after 1 day, 3 days, 1 week
3. **Practice commands:** Open a terminal and try the commands
4. **Teach someone:** Explaining reinforces learning

---

*Quiz based on Linux Bible 10th Edition - Part 1: Getting Started*
