# Viewing Processes

## Key Concepts

| Term | Definition |
|------|------------|
| **Process** | A running instance of a command or program |
| **PID** | Process ID (Unique number for each process) |
| **User/Group** | Account that owns the process |
| **Daemon** | Background system process (often starts at boot) |

---

## Listing Processes with `ps`

The `ps` command shows a snapshot of current processes.

**Common Options:**

| Option | Meaning |
|--------|---------|
| `ps` | Show processes for current shell only |
| `ps u` | Show user, CPU, RAM, and time info |
| `ps aux` | Show **all** processes for **all** users |
| `ps -ef` | Standard syntax for showing all processes full details |

### Understanding `ps u` Output

```
USER   PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
jake  2147  0.0  0.7   1836  1020 tty1     S+   14:50   0:00 -bash
```

*   **USER:** Who started it
*   **PID:** Process ID (Important for killing!)
*   **%CPU/%MEM:** Resources used
*   **VSZ/RSS:** Memory size (Virtual vs Resident/Physical)
*   **STAT:** State (R=Running, S=Sleeping, Z=Zombie, +=Foreground)
*   **COMMAND:** The command that was run

> **Tip:** Use `ps aux | less` to scroll through all processes.

---

## Monitoring with `top`

`top` provides a real-time, dynamic view of running processes.

*   **Updates:** Every 5 seconds by default.
*   **Sorting:**
    *   `P` (Shift+p) = Sort by **CPU** (default)
    *   `M` (Shift+m) = Sort by **Memory**
*   **Actions:**
    *   `k` = Kill a process (enter PID)
    *   `r` = Renice (change priority)
    *   `q` = Quit top

---

## GNOME System Monitor

Graphical tool for viewing processes (like Windows Task Manager).
*   Run `gnome-system-monitor`
*   Right-click processes to Stop, End, Kill, or Change Priority.

---

## Review Questions

1. Which command shows a snapshot of all processes?

2. What does **PID** stand for?

3. Which `top` key sorts processes by memory usage?

4. What does the `RSS` column in `ps` output represent?

5. How do you quit the `top` program?

6. True or False: `ps` shows a real-time updating list of processes.

7. What letter in the STAT column indicates a "Sleeping" process?

8. Which command works like a graphical "Task Manager"?

---

## Quick Reference

```
ps:
├── ps           → Current shell processes
├── ps u         → Detailed info check
└── ps aux       → All users/processes

top:
├── P            → Sort by CPU
├── M            → Sort by Memory
└── k            → Kill process
```
