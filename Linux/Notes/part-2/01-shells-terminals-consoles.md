# Shells, Terminals, and Virtual Consoles

## Key Concepts

| Term | Definition |
|------|------------|
| **Shell** | A command-line interpreter that processes user commands and returns output |
| **Terminal** | An interface (window) that provides access to the shell |
| **Console** | The physical or virtual device for system-level text interaction |
| **TTY** | TeleTYpewriter - a virtual terminal device in Linux |
| **PTY** | Pseudo-Terminal - a terminal emulator used by GUI terminal apps |

---

## Three Ways to Access the Shell

The book describes three common ways to get to a shell interface:

```
┌─────────────────────────────────────────────────────────────┐
│                    Access Methods                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Shell Prompt        2. Terminal Window   3. Virtual     │
│     (Text Login)           (GUI)               Console      │
│  ┌────────────────┐    ┌────────────────┐  ┌────────────┐   │
│  │ Plain text     │    │ GNOME Terminal │  │ Ctrl+Alt+  │   │
│  │ login screen   │    │ in desktop     │  │ F1-F6      │   │
│  │ No GUI needed  │    │ environment    │  │ Text-only  │   │
│  └────────────────┘    └────────────────┘  └────────────┘   │
│           │                    │                 │          │
│           └────────────────────┴─────────────────┘          │
│                              │                              │
│                              ▼                              │
│                    ┌─────────────────────┐                  │
│                    │       SHELL         │                  │
│                    │  (bash, zsh, fish)  │                  │
│                    └─────────────────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 1. Using the Shell Prompt (Text Login)

If your Linux system has no GUI (or it's not working), you'll see a text login prompt:

```
Red Hat Enterprise Linux Server release 8.0 (Ootpa)
Kernel 4.18.0-42.el8.x86_64 on an X86_64
mylinuxhost login:
```

### Default Shell Prompts

After logging in, you'll see a prompt indicating the shell is ready:

```bash
# Regular user prompt
$

# Root user prompt
#

# Full prompt format (typical)
[username@hostname directory]$

# Example
[jake@pine share]$
#  │      │    │
#  │      │    └── Current directory (basename)
#  │      └── Hostname
#  └── Username
```

> **Note:** Although `#` indicates a root command, you typically use `sudo` instead of logging in as root directly. See Chapter 8 for more on `sudo`.

---

## 2. Using a Terminal Window (GUI)

With a desktop GUI running, you open a Terminal emulator to access the shell.

### How to Open a Terminal

**Method 1: Right-click the desktop**
- Look for: "Open in Terminal", "Shells", "Terminal Window", or "Xterm"
- (Some distributions have disabled this feature)

**Method 2: Application menu**
- **GNOME 2:** Applications → System Tools → Terminal
- **GNOME 3/4:** Click Activities → Type "Terminal" → Press Enter
- **KDE:** Application Menu → System → Konsole

### GNOME Terminal Features

The default terminal in Fedora/RHEL is **GNOME Terminal** (`gnome-terminal` command).

| Feature | How to Access |
|---------|---------------|
| **Change Font** | Edit → Preferences → Profile → Custom font |
| **Change Colors** | Edit → Preferences → Colors (uncheck "Use system theme") |
| **Multiple Tabs** | Ctrl+Shift+T or File → New Tab |
| **Scrollback** | Edit → Preferences → Scrolling |
| **Profiles** | Edit → Preferences → Profiles |

### Customizing GNOME Terminal

1. Open Terminal: Activities → Terminal
2. Go to Edit → Preferences (or Profile Preferences)
3. On General/Profile tab: Check "Custom font", select a new font
4. On Colors tab: Uncheck "Use colors from system theme" to customize
5. Adjust scrollback buffer in Scrolling settings

### Terminal Emulators

| Terminal | Desktop | Features |
|----------|---------|----------|
| **GNOME Terminal** | GNOME | Tabs, profiles, configurable (default on Fedora/RHEL) |
| **Konsole** | KDE | Tabs, split view, bookmarks |
| **Alacritty** | Any | GPU-accelerated, fast, minimal |
| **Kitty** | Any | GPU-accelerated, ligatures, images |
| **xterm** | X11 | Classic, lightweight |

### Terminal vs Shell

```
Terminal Emulator              Shell
─────────────────             ─────
- The window                  - The program running inside
- Handles display, fonts      - Interprets commands
- Keyboard input              - Executes programs
- Copy/paste, colors          - Scripting, aliases

Example: GNOME Terminal (terminal) running bash (shell)
```

---

## 3. Using Virtual Consoles

Virtual consoles are a way to have **multiple shell sessions open at once** in addition to the graphical interface. They run independent of the GUI.

### When Virtual Consoles Are Useful

- The GUI crashes or freezes
- Need a clean environment for troubleshooting
- Running system-level tasks
- Server administration (no GUI available)
- Want multiple independent sessions

### Switching Virtual Consoles

```bash
# Switch to virtual console (hold Ctrl+Alt, press function key)
Ctrl + Alt + F1    # TTY1 - Often GDM login screen (Fedora 29+)
Ctrl + Alt + F2    # TTY2 - First desktop session (Fedora 29+)
Ctrl + Alt + F3    # TTY3 - Text console (or second desktop)
Ctrl + Alt + F4    # TTY4 - Text console
Ctrl + Alt + F5    # TTY5 - Text console
Ctrl + Alt + F6    # TTY6 - Text console

# Return to GUI
Ctrl + Alt + F1    # or F2, varies by distribution
```

### Modern Distribution Layout (Fedora 29+)

```
Virtual Console Layout:
├── TTY1 (Ctrl+Alt+F1) → GDM login screen (persistent)
├── TTY2 (Ctrl+Alt+F2) → First desktop session
├── TTY3 (Ctrl+Alt+F3) → Second desktop session (or text)
├── TTY4-F6            → Additional text consoles
```

> **Note:** Newer systems like Fedora 29+ start GDM persistently on tty1, allowing multiple simultaneous GUI sessions.

### Try It Now

1. Press **Ctrl+Alt+F3** - you should see a plain-text login prompt
2. Log in with your username and password
3. Try a few commands (ls, pwd, whoami)
4. Type `exit` to log out
5. Press **Ctrl+Alt+F1** or **Ctrl+Alt+F2** to return to GUI

### TTY Commands

```bash
# Check which TTY you're on
tty
# Output: /dev/tty3 (virtual console)
# or: /dev/pts/0 (pseudo-terminal in GUI)

# See who is logged in on which TTY
who
# Output:
# jake     tty2         2024-01-15 10:00 (:0)
# jake     tty3         2024-01-15 10:05

# List TTY devices
ls /dev/tty*
```

---

## Shell Types

### Available Shells

| Shell | Full Name | Description |
|-------|-----------|-------------|
| **bash** | Bourne Again SHell | Default on most Linux distros, POSIX-compliant |
| **zsh** | Z Shell | Feature-rich, default on macOS, highly customizable |
| **fish** | Friendly Interactive SHell | User-friendly, syntax highlighting, not POSIX-compliant |
| **sh** | Bourne Shell | Original Unix shell, minimal |
| **dash** | Debian Almquist Shell | Fast, minimal, used for system scripts |
| **ksh** | Korn Shell | Advanced scripting features |

### Check Your Shell

```bash
# Display default shell
echo $SHELL

# Display currently running shell
echo $0

# List all available shells on system
cat /etc/shells

# Change your default shell
chsh -s /bin/zsh
```

---

## Login vs Non-Login Shell

```
Login Shell:
├── Started when you physically log in
├── Console login, SSH connection, su -l
├── Reads: /etc/profile, ~/.bash_profile
├── Example: ssh user@server

Non-Login Shell:
├── Started from within another session
├── Opening terminal window in GUI
├── Reads: ~/.bashrc only
├── Example: Opening GNOME Terminal in desktop
```

### Check if Login Shell

```bash
# Method 1: Check $0
echo $0
# -bash (with dash) = login shell
# bash (no dash) = non-login shell

# Method 2: Using shopt
shopt -q login_shell && echo "Login shell" || echo "Non-login shell"
```

---

## Shell Configuration Files

```bash
# Bash configuration files (order of execution)

# System-wide (all users)
/etc/profile           # Login shell - system-wide settings
/etc/bash.bashrc       # Interactive non-login shell (some distros)

# User-specific
~/.bash_profile        # Login shell (or ~/.profile)
~/.bashrc              # Interactive non-login shell - YOUR customizations go here
~/.bash_logout         # Executed on logout
```

### Best Practice

Put all your customizations in `~/.bashrc`, and have `~/.bash_profile` source it:

```bash
# ~/.bash_profile
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

---

## Review Questions

1. What are the three common ways to access a shell in Linux?

2. What is the default prompt for a regular user and for the root user?

3. How do you open a Terminal window in GNOME 3?

4. What keyboard shortcut switches to virtual console 3?

5. What is the difference between a terminal and a shell?

6. On modern Fedora systems, which TTY runs the GDM login screen?

7. What command shows which TTY you are currently using?

8. What is the difference between a login shell and a non-login shell?

---

## Quick Reference

```
Accessing the Shell:
├── Text login          → Log in at console prompt
├── Terminal window     → Activities → Terminal (GNOME)
└── Virtual console     → Ctrl+Alt+F1-F6

Shell Prompts:
├── $    → Regular user
├── #    → Root user
└── [user@host dir]$ → Full format

Virtual Consoles:
├── Ctrl+Alt+F1    → GDM/GUI (varies)
├── Ctrl+Alt+F2    → First desktop
├── Ctrl+Alt+F3-F6 → Text consoles
└── tty            → Show current TTY

Shell Commands:
├── echo $SHELL       → Default shell
├── echo $0           → Current shell
├── cat /etc/shells   → Available shells
├── chsh -s /bin/zsh  → Change shell
├── who               → Logged-in users
└── tty               → Current terminal

Config Files (bash):
├── /etc/profile      → System login
├── ~/.bash_profile   → User login
├── ~/.bashrc         → User non-login (customize here!)
└── ~/.bash_logout    → Logout cleanup
```
