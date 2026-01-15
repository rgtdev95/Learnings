# Part 1: Getting Started with Linux

> Based on Linux Bible, 10th Edition by Christopher Negus

## Topics Overview

This section introduces the fundamentals of Linux - what it is, where it came from, and why it matters.

---

## Study Materials

### 📚 Detailed Notes

| # | Topic | Notes File |
|---|-------|------------|
| 1 | Understanding What Linux Is | [01-understanding-linux.md](../Notes/part-1/01-understanding-linux.md) |
| 2 | How Linux Differs from Other OS | [02-linux-vs-other-os.md](../Notes/part-1/02-linux-vs-other-os.md) |
| 3 | Linux History | [03-linux-history.md](../Notes/part-1/03-linux-history.md) |
| 4 | How Linux Distributions Emerged | [04-linux-distributions.md](../Notes/part-1/04-linux-distributions.md) |
| 5 | How Companies Make Money with Linux | [05-linux-business-model.md](../Notes/part-1/05-linux-business-model.md) |

### ❓ Quiz & Flashcards

- [Part 1 Quiz](../Quiz/Part-1/part-1-quiz.md) - 30 questions with collapsible answers

### 📖 Additional Reference

- [Understanding What Linux Is (Extended)](../understanding-what-linux-is.md) - Shell, commands, and environment

---

## Learning Objectives

After completing Part 1, you should be able to:

- [ ] Explain what Linux is and its core components
- [ ] Describe how Linux differs from Windows and macOS
- [ ] Summarize key events in Linux history
- [ ] Identify major distribution families (Debian, Red Hat, Arch)
- [ ] Understand how companies profit from open source software

---

## Key Commands Covered

```bash
# System Information
uname -r                    # Kernel version
uname -a                    # Full system info
cat /etc/os-release         # Distribution info
hostname                    # System hostname

# Resource Viewing
free -h                     # Memory usage
df -h                       # Disk usage
lscpu                       # CPU information

# File Permissions
ls -la                      # List with permissions
chmod 755 file              # Change permissions
sudo command                # Run as root
```

---

## Quick Summary

| Topic | Key Takeaway |
|-------|--------------|
| What is Linux | Free, open-source kernel created in 1991 |
| vs Other OS | Open source, customizable, secure, free |
| History | Unix → GNU → Linux → Distributions |
| Distributions | Debian, Red Hat, Arch families |
| Business Models | Support, cloud, certifications |

---

## Next Steps

After mastering Part 1, proceed to:
- **Part 2:** [Linux Power User](./part-2-linux-power-user.md) - Shell, files, processes, scripts