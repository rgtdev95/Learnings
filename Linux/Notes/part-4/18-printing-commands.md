# Printing Commands

## The lp Command

**Purpose:** Print files to local or remote printers

**Basic syntax:**
```bash
lp filename
```

**Print to specific printer:**
```bash
lp -d printername filename
```

**Using pipes:**
```bash
cat file.txt | lp
echo "Hello" | lp
```

---

## lp Command Options

**Number of copies:**
```bash
lp -n 5 file.txt          # Print 5 copies
lp -# 5 file.txt          # Alternative syntax
```

**Raw mode (pre-formatted):**
```bash
lp -o raw file.ps         # Send without formatting
```

**Page range:**
```bash
lp -P 1-10 document.pdf   # Print pages 1-10
```

**Priority:**
```bash
lp -q 50 file.txt         # Set priority (1-100)
```

**Job name:**
```bash
lp -t "My Document" file.txt
```

---

## Setting Default Printer

**For current user:**

Add to `~/.bashrc`:
```bash
export PRINTER=lp3
```

**System-wide:**

Edit `/etc/cups/lpoptions`:
```
Default lp3
```

**Using lpadmin:**
```bash
lpadmin -d printername
```

---

## lpstat Command

**Purpose:** Show printer status

**Show all printers:**
```bash
lpstat -p
```

**Show default printer:**
```bash
lpstat -d
```

**Show all status:**
```bash
lpstat -t
```

**Example output:**
```
printer hp disabled since Wed 10 Jul 2019 10:53:34 AM EDT
printer deskjet-555 is idle. enabled since Wed 10 Jul 2019
10:53:34 AM EDT
```

**Show print jobs:**
```bash
lpstat -o              # All jobs
lpstat -o printername  # Jobs for specific printer
```

---

## lpq Command

**Purpose:** View print queue

**View default printer queue:**
```bash
lpq
```

**View specific printer:**
```bash
lpq -P printername
```

**Example output:**
```
printer is ready and printing
Rank  Owner    Job  Files          Total Size  Time
active root    133  /home/jake/pr1 467         10:30:45
2      root    197  /home/jake/doc 23948       10:31:12
```

**Columns:**
*   **Rank:** Position in queue (active = printing)
*   **Owner:** User who submitted job
*   **Job:** Job number
*   **Files:** Filename
*   **Total Size:** File size in bytes
*   **Time:** When job was submitted

---

## lprm Command

**Purpose:** Remove print jobs from queue

**Remove all your jobs (default printer):**
```bash
lprm
```

**Remove all your jobs (specific printer):**
```bash
lprm -P printername
```

**Remove all your jobs (all printers):**
```bash
lprm -
```

**Remove specific job:**
```bash
lprm 133               # Remove job 133
```

**Remove all jobs for a user (root only):**
```bash
lprm -U username
```

**Example:**
```bash
# View queue
lpq
# Remove job 133
lprm 133
```

---

## Managing Print Queues

### cupsenable / cupsdisable

**Enable printer:**
```bash
cupsenable printername
```

**Disable printer:**
```bash
cupsdisable printername
```

**Disable with reason:**
```bash
cupsdisable -r "Out of toner" printername
```

### cupsaccept / cupsreject

**Accept new jobs:**
```bash
cupsaccept printername
```

**Reject new jobs:**
```bash
cupsreject printername
```

**Reject with reason:**
```bash
cupsreject -r "Printer maintenance" printername
```

**Difference:**
*   **disable:** Stop printing, but accept jobs to queue
*   **reject:** Don't accept new jobs, but continue printing queue

---

## lpadmin Command

**Purpose:** Administer printers

**Add printer:**
```bash
lpadmin -p printername -E -v device-uri -m ppd-file
```

**Set default printer:**
```bash
lpadmin -d printername
```

**Delete printer:**
```bash
lpadmin -x printername
```

**Set printer description:**
```bash
lpadmin -p printername -D "HP LaserJet in Room 205"
```

**Set location:**
```bash
lpadmin -p printername -L "Room 205"
```

**Enable printer:**
```bash
lpadmin -p printername -E
```

---

## Printer Variables

**PRINTER variable:**
```bash
export PRINTER=hp-laserjet
```

**LPDEST variable (alternative):**
```bash
export LPDEST=hp-laserjet
```

**Priority:**
1. `-d` option on command line
2. `PRINTER` environment variable
3. `LPDEST` environment variable
4. System default printer

---

## Print Job Control

**Hold a job:**
```bash
lp -H hold filename
```

**Release a held job:**
```bash
lp -H resume -i job-id
```

**Modify job priority:**
```bash
lp -i job-id -q 75
```

**Cancel job:**
```bash
cancel job-id
```

---

## Checking Printer Configuration

**List printers:**
```bash
lpstat -p -d
```

**Show printer details:**
```bash
lpoptions -p printername -l
```

**Show default options:**
```bash
lpoptions -d
```

---

## Review Questions

1. What command prints a file to the default printer?

2. How do you print 10 copies of a file?

3. What does `lpstat -t` show?

4. How do you remove job number 150 from the queue?

5. What's the difference between cupsdisable and cupsreject?

6. How do you set the default printer for your user?

7. What command shows the print queue?

8. True or False: lprm can only remove your own jobs.

---

## Quick Reference

```
Print Commands:
├── lp file              → Print to default printer
├── lp -d printer file   → Print to specific printer
├── lp -n 5 file         → Print 5 copies
└── lp -o raw file       → Raw mode

View Status:
├── lpstat -t            → All status
├── lpstat -p            → Printer status
├── lpstat -d            → Default printer
└── lpstat -o            → Print jobs

View Queue:
├── lpq                  → Default printer queue
└── lpq -P printer       → Specific printer queue

Remove Jobs:
├── lprm                 → Remove all your jobs
├── lprm 133             → Remove job 133
├── lprm -               → Remove all your jobs (all printers)
└── lprm -U user         → Remove user's jobs (root only)

Manage Queues:
├── cupsenable printer   → Enable printing
├── cupsdisable printer  → Disable printing
├── cupsaccept printer   → Accept new jobs
└── cupsreject printer   → Reject new jobs

Administration:
├── lpadmin -d printer   → Set default
├── lpadmin -p name -E   → Add/enable printer
└── lpadmin -x name      → Delete printer

Default Printer:
├── export PRINTER=name
├── lpadmin -d name
└── Edit ~/.bashrc

Job Control:
├── lp -H hold file      → Hold job
├── lp -H resume -i id   → Release job
└── cancel job-id        → Cancel job
```
