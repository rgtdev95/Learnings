# Memory Troubleshooting

## Virtual Memory Overview

**Storage types:**

**Hard disk (permanent):**
*   Large capacity (TB)
*   Slow access
*   Permanent storage

**RAM (temporary):**
*   Smaller capacity (GB)
*   Fast access
*   Near CPU
*   More expensive
*   Lost when powered off

**Swap space (extended RAM):**
*   On hard disk
*   Slower than RAM
*   Extends available memory
*   Not permanent storage

---

## How Swap Works

**Swapping out:**
*   Data moved from RAM to swap
*   Performance hit (disk is slower)
*   Happens when RAM is full

**Swapping in:**
*   Data returned from swap to RAM
*   Another performance hit
*   Happens when data is needed

**Effects:**
*   System stays running
*   Performance degrades
*   Like losing high gear in a car

**Out of Memory (OOM):**
*   Both RAM and swap full
*   Kernel OOM killer activates
*   Processes killed to free memory

---

## Swappiness

**Definition:** How aggressively system swaps

**Arguments:**
*   **Pro:** Swap inactive data, free RAM for active processes
*   **Con:** Performance hit when swapped data needed

**Example:**
*   Document minimized for days
*   Swap it out to free RAM
*   Performance hit when reopened

---

## Checking Memory Usage

**Using top command:**
```bash
top
```

**Press M to sort by memory**

**Key lines:**
```
Mem:  3716196k total, 2684924k used, 1031272k free
Swap: 4194296k total,       0k used, 4194296k free
```

**Key columns:**
*   `VIRT` - Virtual memory allocated
*   `RES` - Resident (non-swappable) memory used
*   `SHR` - Shared memory
*   `%MEM` - Percentage of total memory

**Example output:**
```
PID  USER   VIRT   RES   SHR  %MEM COMMAND
6679 user  1665m  937m   32m  25.8 firefox
6794 user   743m  181m   30m   5.0 npviewer.bin
```

---

## Signs of Memory Problems

**Low memory:**
*   Free space on Mem line near zero
*   Swap usage increasing
*   System performance degrading

**Memory leak:**
*   VIRT memory growing for process
*   RES memory continuously growing
*   Process never releases memory

**Out of memory:**
*   Swap space exhausted
*   Kernel kills processes
*   System becomes unresponsive

---

## Using Cockpit for Monitoring

**Access Cockpit:**
```
https://localhost:9090
```

**View memory:**
*   System → Memory & Swap
*   Real-time graphs
*   RAM usage
*   Swap usage

---

## Dealing with Memory Problems

### Kill a Process

**From top:**
1. Press `k`
2. Enter PID
3. Choose signal (15 or 9)

**From command line:**
```bash
kill -15 PID  # Graceful
kill -9 PID   # Force
```

### Drop Page Caches

**Temporary relief:**
```bash
echo 3 > /proc/sys/vm/drop_caches
```

**Effect:**
*   Writes inactive pages to disk
*   Discards cached data
*   Frees up RAM
*   Like cleaning your desk

### Kill OOM Process

**Enable Alt+SysRq:**

Add to `/etc/sysctl.conf`:
```
kernel.sysrq = 1
```

**From text console (Ctrl+Alt+F2):**
*   Press `Alt+SysRq+f`
*   Kills highest OOM score process
*   Repeat as needed

**Other Alt+SysRq keys:**
*   `Alt+SysRq+e` - Terminate all processes
*   `Alt+SysRq+t` - Dump task list
*   `Alt+SysRq+b` - Reboot system

---

## Long-Term Solutions

**Add capacity:**
*   Install more RAM
*   Add swap space
*   Upgrade CPU
*   Increase disk space

**Tune the system:**
*   Adjust swappiness
*   Modify kernel parameters
*   Optimize applications

**Fix problem applications:**
*   Find memory leaks
*   Fix broken applications
*   Limit resource usage
*   Educate users

---

## Memory Commands

**View memory:**
```bash
free -h          # Human-readable
top              # Interactive
htop             # Enhanced top
```

**View swap:**
```bash
swapon -s        # Show swap devices
cat /proc/swaps  # Swap information
```

**Add swap:**
```bash
# Create swap file
dd if=/dev/zero of=/swapfile bs=1M count=1024
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

---

## Review Questions

1. What's the difference between RAM and swap?

2. What happens when both RAM and swap are full?

3. What does the RES column in top show?

4. How do you sort top by memory usage?

5. What command drops page caches?

6. How do you enable Alt+SysRq keys?

7. What does Alt+SysRq+f do?

8. True or False: Swapping improves performance.

---

## Quick Reference

```
Memory Types:
├── Hard disk: Permanent, slow, large
├── RAM: Temporary, fast, small, expensive
└── Swap: Extended RAM, on disk, slower

top Command:
├── Run: top
├── Sort by memory: M
├── Kill process: k
└── Quit: q

Memory Columns:
├── VIRT → Virtual memory allocated
├── RES  → Resident memory used
├── SHR  → Shared memory
└── %MEM → Percentage of total

Signs of Problems:
├── Free RAM near zero
├── Swap usage increasing
├── RES memory growing (leak)
└── System slowing down

Quick Fixes:
├── Kill process: kill -9 PID
├── Drop caches: echo 3 > /proc/sys/vm/drop_caches
└── Alt+SysRq+f: Kill OOM process

Alt+SysRq Keys:
├── Enable: kernel.sysrq = 1
├── f → Kill OOM process
├── e → Terminate all
├── t → Dump tasks
└── b → Reboot

View Memory:
├── free -h
├── top
├── htop
└── cat /proc/meminfo

Cockpit:
└── https://localhost:9090 → System → Memory & Swap
```
