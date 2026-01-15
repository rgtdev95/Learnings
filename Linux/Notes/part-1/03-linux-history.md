# Linux History

## Key Concepts

| Year | Event | Significance |
|------|-------|--------------|
| **1969** | Unix created at Bell Labs | Foundation for all Unix-like systems |
| **1983** | GNU Project started | Free software movement begins |
| **1991** | Linux kernel released | Linus Torvalds creates Linux |
| **1992** | Linux goes GPL | Becomes truly free and open |
| **1993** | Debian & Slackware | First major distributions |
| **1994** | Linux 1.0 released | First production-ready version |

---

## The Unix Foundation (1969-1980s)

### Bell Labs and the Birth of Unix

```
Timeline:
1969 ─── Ken Thompson & Dennis Ritchie create Unix at Bell Labs
         │
1971 ─── First Unix manual published
         │
1973 ─── Unix rewritten in C language (portable!)
         │
1977 ─── BSD (Berkeley Software Distribution) created
         │
1983 ─── AT&T releases commercial Unix System V
```

**Key Unix Innovations:**
- Hierarchical file system
- Pipes for connecting programs
- Shell scripting
- "Everything is a file" philosophy
- Multi-user, multi-tasking capability

### The Unix Family Tree

```
                    Original Unix (Bell Labs)
                           │
            ┌──────────────┼──────────────┐
            │              │              │
        BSD Unix      System V      Commercial Unix
            │              │              │
     ┌──────┴──────┐       │        ┌─────┴─────┐
     │             │       │        │           │
  FreeBSD      OpenBSD   Linux   Solaris     AIX
     │             │    (clone)    (Sun)    (IBM)
  macOS         Security
  (Darwin)      focused
```

---

## The GNU Project (1983)

### Richard Stallman's Vision

**GNU = "GNU's Not Unix"** (recursive acronym)

```
The Four Freedoms of Free Software:
├── Freedom 0: Run the program for any purpose
├── Freedom 1: Study and modify the source code
├── Freedom 2: Redistribute copies
└── Freedom 3: Distribute modified versions
```

**GNU provided essential tools:**
- GCC (GNU Compiler Collection)
- GNU Bash shell
- GNU Core Utilities (ls, cp, mv, etc.)
- GNU Emacs editor
- GLIBC (C library)

**The Missing Piece:** GNU needed a kernel. The GNU Hurd kernel was incomplete.

---

## Linux is Born (1991)

### Linus Torvalds and the First Announcement

On August 25, 1991, Linus Torvalds posted this message:

> "Hello everybody out there using minix - I'm doing a (free) operating system (just a hobby, won't be big and professional like gnu) for 386(486) AT clones."

**Key Facts:**
- Created as a personal project by Finnish student Linus Torvalds
- Originally ran only on Intel 386 processors
- Inspired by MINIX (educational Unix clone)
- Released source code freely on the internet

### Why Linux Succeeded

| Factor | Description |
|--------|-------------|
| **Timing** | Arrived when GNU tools existed but lacked a kernel |
| **Licensing** | GPL allowed commercial and community use |
| **Internet** | Email and Usenet enabled global collaboration |
| **Architecture** | Modular design allowed contribution |
| **Community** | Welcoming environment for developers |

---

## Evolution of Linux (1992-Present)

### Major Milestones

```
1992 ─── Linux adopts GPL license
         └── Opens up commercial possibilities
         
1993 ─── First distributions appear
         ├── Slackware (still active!)
         └── Debian (foundation for Ubuntu)
         
1994 ─── Linux 1.0 released
         └── First "stable" release
         
1996 ─── Linux 2.0 with SMP support
         └── Multiple processors supported
         
1999 ─── Linux 2.2 - Enterprise ready
         └── Better networking, large file support
         
2003 ─── Linux 2.6 - Major rewrite
         └── Scalability improvements
         
2011 ─── Linux 3.0 (renumbering)
         └── Just a version change, no major features
         
2015 ─── Linux 4.0 with live patching
         └── Update kernel without reboot
         
2019 ─── Linux 5.0 released
         └── Continued improvements
         
2022 ─── Linux 6.0 released
         └── Modern features, Rust support added
```

### Linux Kernel Version Numbers

```bash
# Check your kernel version
uname -r

# Example output: 5.15.0-91-generic
#                 │  │  │  │
#                 │  │  │  └── Distribution-specific identifier
#                 │  │  └── Patch level
#                 │  └── Minor version
#                 └── Major version
```

---

## Key Figures in Linux History

| Person | Contribution |
|--------|--------------|
| **Linus Torvalds** | Created Linux kernel, still maintains it |
| **Richard Stallman** | Started GNU Project, wrote GPL license |
| **Ken Thompson** | Co-created Unix at Bell Labs |
| **Dennis Ritchie** | Co-created Unix, invented C language |
| **Andrew Tanenbaum** | Created MINIX, inspired Linux |
| **Ian Murdock** | Created Debian distribution |
| **Patrick Volkerding** | Created Slackware distribution |

---

## Practical Examples

### Explore Your Linux History
```bash
# View kernel version and release date
uname -a

# Check distribution release info
cat /etc/os-release

# View kernel release history (if available)
cat /usr/share/doc/linux-base/changelog.gz | zless

# See kernel compile date
cat /proc/version
```

### Understanding Your System's Heritage
```bash
# Is your distro Debian-based?
apt --version 2>/dev/null && echo "Debian-based"

# Is your distro Red Hat-based?
dnf --version 2>/dev/null && echo "Red Hat-based"

# Check init system (systemd or other)
ps -p 1 -o comm=
```

---

## Review Questions

1. Who created Unix and in what year?

2. What does GNU stand for and who started the project?

3. When did Linus Torvalds release the first Linux kernel?

4. Why was Linux created as a "hobby project"?

5. What role did the GPL license play in Linux's success?

6. Name two tools that came from the GNU Project.

7. What is the relationship between Unix, GNU, and Linux?

8. What command shows the kernel version on your system?

---

## Timeline Summary

```
        1969          1983          1991          Today
          │             │             │             │
    Unix Created   GNU Started   Linux Born   Linux Everywhere
          │             │             │             │
    ┌─────┴─────┐       │       ┌─────┴─────┐       │
    │           │       │       │           │       │
  Bell Labs   BSD    Free      Hobby    Enterprise
   Research  at UC   Software  Project  Standard
            Berkeley Movement
```
