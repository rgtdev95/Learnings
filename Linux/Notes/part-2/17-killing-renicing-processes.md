# Killing and Renicing Processes

## Key Concepts

| Term | Definition |
|------|------------|
| **Signal** | Message sent to a process (Start, Stop, Die) |
| **SIGTERM (15)** | Polite kill signal (Default). Asks to stop. |
| **SIGKILL (9)** | Force kill signal. Immediate stop. |
| **Nice Value** | Priority level (-20 to 19). Lower = Higher Priority. |

---

## Killing Processes (`kill`)

The `kill` command sends signals to a specific **PID**.

### Common Signals

| Number | Name | Description |
|--------|------|-------------|
| **15** | SIGTERM | Terminate gracefully (cleanup allowed) |
| **9** | SIGKILL | Kill immediately (no cleanup) |
| **1** | SIGHUP | Hangup (Reload config) |
| **19** | SIGSTOP | Pause process (cannot be blocked) |
| **18** | SIGCONT | Continue paused process |

### commands

```bash
kill 1234          # Send SIGTERM (15) to PID 1234
kill -9 1234       # Send SIGKILL (9) to PID 1234
kill -SIGHUP 1234  # Send SIGHUP to PID 1234
```

---

## Killing by Name (`killall`)

Kills **all** processes matching the name. Be careful!

```bash
killall firefox    # Kills all firefox processes
killall -9 bash    # Kills ALL usage of bash (Dangerous!)
```

---

## Process Priority (`nice` / `renice`)

Priority determines CPU time access.
*   Range: **-20** (Highest Priority) to **19** (Lowest Priority).
*   Default: **0**.
*   **Rules:** Standard users can only **lower** priority (increase number). Root can do anything.

### Starting with Priority (`nice`)

```bash
nice -n 10 backup.sh      # Run with lower priority (10)
nice -n -5 important.sh   # Run with higher priority (-5) [Root only]
```

### Changing Priority (`renice`)

Changes priority of an **existing** PID.

```bash
renice 5 1234             # Set PID 1234 priority to 5
renice -n -5 1234         # Set PID 1234 priority to -5 [Root only]
```

---

## Review Questions

1. Which signal number is SIGKILL?

2. Which signal allows a process to clean up before exiting?

3. What command kills processes by name instead of PID?

4. Is a Nice value of `-10` higher or lower priority than `10`?

5. Can a regular user set a Nice value of `-5`?

6. What does `kill -1` (SIGHUP) often make a daemon do?

7. Which command changes the priority of a running process?

8. Why is `kill -9` considered a "last resort"?

---

## Quick Reference

```
Killing:
├── kill <PID>       → Terminate (15)
├── kill -9 <PID>    → Force Kill (9)
└── killall <name>   → Kill by name

Priority:
├── nice -n 5 cmd    → Start with priority
└── renice 5 <PID>   → Change priority
```
