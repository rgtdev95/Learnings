# Background and Foreground Jobs

## Key Concepts

| Term | Definition |
|------|------------|
| **Foreground** | Updates the screen, takes keyboard input (blocks shell) |
| **Background** | Runs invisible to shell, leaves prompt free |
| **Job ID** | Shell's reference number for the process (e.g., `[1]`) |
| **`&`** | Operator to run command in background |

---

## Managing Jobs

### Starting in Background (`&`)
Add `&` to the end of a command to run it in the background immediately.

```bash
find / -name "*.log" > logfiles.txt &
# Output: [1] 12345
# [Job ID] PID
```

### Stopping (Pausing) a Job (`Ctrl+Z`)
If a command is running in the foreground and you want to pause it to get your prompt back:

1.  Press `Ctrl+Z`
2.  Shell says: `[1]+ Stopped command_name`

### Viewing Jobs (`jobs`)
Lists all jobs started from the current shell.

```bash
$ jobs
[1]- Stopped       vi myfile.txt
[2]+ Running       sleep 100 &
```

*   `+` = Most recent job
*   `-` = Previous job

### Foreground (`fg`) and Background (`bg`)

*   **`fg`**: Brings a background/stopped job to the **foreground**.
    *   `fg %1` (Brings job [1] to front)
*   **`bg`**: Resumes a stopped job in the **background**.
    *   `bg %1` (Starts job [1] running in background)

---

## Common Workflow

1.  Run `vi script.sh` (Foreground)
2.  Realize you need to check a file.
3.  Press `Ctrl+Z` (Stops vi, gives prompt back).
4.  Run `ls -l` (Check file).
5.  Type `fg` (Brings vi back to foreground to finish editing).

---

## Review Questions

1. What symbol do you add to a command to run it in the background?

2. How do you pause a running foreground process?

3. Which command lists your current shell jobs?

4. How do you bring Job 2 to the foreground?

5. What happens if you close the terminal while background jobs are running? (Usually they terminate)

6. True or False: `Ctrl+Z` kills the process.

7. What does the `+` sign mean in the `jobs` list?

8. If you `bg` a stopped job, what happens?

---

## Quick Reference

```
Control:
├── cmd &        → Run in background
├── Ctrl+Z       → Pause (Stop)
├── Ctrl+C       → Kill (Interrupt)

Commands:
├── jobs         → List jobs
├── fg %1        → Bring job 1 to front
└── bg %1        → Resume job 1 in back
```
