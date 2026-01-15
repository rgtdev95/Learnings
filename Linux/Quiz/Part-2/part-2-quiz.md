# Part 2: Linux Power User - Quiz & Flashcards

> **How to use:** Click on "Answer" to reveal the answer. Test yourself before revealing!

---

## Topic 1: Shells, Terminals, and Consoles

**Q1:** What is the difference between a shell and a terminal?
<details>
<summary>Answer</summary>

- **Terminal:** The window/interface that provides access to the shell (e.g., GNOME Terminal, Konsole)
- **Shell:** The command interpreter running inside the terminal (e.g., bash, zsh, fish)

The terminal handles display and input; the shell interprets and executes commands.

</details>

---

**Q2:** Name three different shells and their characteristics.
<details>
<summary>Answer</summary>

1. **bash** - Bourne Again SHell, default on most Linux distros, POSIX-compliant
2. **zsh** - Z Shell, advanced features, default on macOS, highly customizable
3. **fish** - Friendly Interactive SHell, user-friendly, syntax highlighting, not POSIX-compliant

</details>

---

**Q3:** How do you switch to virtual console 3 and back to GUI?
<details>
<summary>Answer</summary>

```bash
Ctrl + Alt + F3    # Switch to TTY3
Ctrl + Alt + F7    # Return to GUI (may vary by distro)
```

</details>

---

**Q4:** What is the difference between login and non-login shells?
<details>
<summary>Answer</summary>

**Login shell:**
- Started when logging in (SSH, console)
- Reads `/etc/profile` and `~/.bash_profile`

**Non-login shell:**
- Started from within another session
- Reads `~/.bashrc`
- Example: Opening terminal in GUI

</details>

---

## Topic 2: Running Commands

**Q5:** What is the basic syntax structure of a Linux command?
<details>
<summary>Answer</summary>

```
command [options] [arguments]
```

Examples:
- `ls` - command only
- `ls -l` - command + option
- `ls -la /home` - command + options + argument

</details>

---

**Q6:** In what order does the shell search for commands?
<details>
<summary>Answer</summary>

1. **Aliases** - User-defined shortcuts
2. **Shell Keywords** - if, then, for, while
3. **Functions** - User-defined functions
4. **Built-ins** - cd, echo, export
5. **Hash Table** - Previously found commands
6. **PATH** - External commands in PATH directories

</details>

---

**Q7:** What is the difference between `which` and `whereis`?
<details>
<summary>Answer</summary>

- **which:** Finds executable location in PATH
  ```bash
  which python
  # /usr/bin/python
  ```

- **whereis:** Finds binary, source, and man page locations
  ```bash
  whereis python
  # python: /usr/bin/python /usr/lib/python3.10 /usr/share/man/man1/python.1.gz
  ```

</details>

---

**Q8:** How do you check the exit status of the last command?
<details>
<summary>Answer</summary>

```bash
echo $?
```

- `0` = success
- Non-zero = error/failure

</details>

---

## Topic 3: Command History and Editing

**Q9:** How do you repeat the last command?
<details>
<summary>Answer</summary>

```bash
!!
```

Common use: forgot sudo
```bash
apt update          # Permission denied
sudo !!             # Runs: sudo apt update
```

</details>

---

**Q10:** What does `!$` represent?
<details>
<summary>Answer</summary>

The **last argument** of the previous command.

```bash
vim /etc/nginx/nginx.conf
cat !$    # Runs: cat /etc/nginx/nginx.conf
```

</details>

---

**Q11:** What keyboard shortcut starts reverse history search?
<details>
<summary>Answer</summary>

`Ctrl + R`

Then type to search, press `Ctrl+R` again for older matches, `Enter` to run.

</details>

---

**Q12:** What do these key combinations do?
- `Ctrl + A`
- `Ctrl + E`
- `Ctrl + U`
- `Ctrl + K`

<details>
<summary>Answer</summary>

- `Ctrl + A` - Move cursor to **beginning** of line
- `Ctrl + E` - Move cursor to **end** of line
- `Ctrl + U` - Delete from cursor to **beginning** of line
- `Ctrl + K` - Delete from cursor to **end** of line

</details>

---

## Topic 4: Connecting and Expanding Commands

**Q13:** What does the pipe operator (`|`) do?
<details>
<summary>Answer</summary>

Connects the **stdout** (standard output) of one command to the **stdin** (standard input) of another.

```bash
ls -l | head -10    # First 10 lines of ls output
cat file.txt | grep "error"
```

</details>

---

**Q14:** What is the difference between `;`, `&&`, and `||`?
<details>
<summary>Answer</summary>

- `;` - Run commands **sequentially** (regardless of success/failure)
- `&&` - Run next command only if previous **succeeds**
- `||` - Run next command only if previous **fails**

```bash
cmd1; cmd2       # Both run
cmd1 && cmd2     # cmd2 runs only if cmd1 succeeds
cmd1 || cmd2     # cmd2 runs only if cmd1 fails
```

</details>

---

**Q15:** How do you run a command in the background?
<details>
<summary>Answer</summary>

```bash
command &
```

Job control:
- `Ctrl+Z` - Suspend current process
- `bg` - Resume in background
- `fg` - Bring to foreground
- `jobs` - List background jobs

</details>

---

**Q16:** What does `2>&1` mean?
<details>
<summary>Answer</summary>

Redirect **stderr (2)** to the same place as **stdout (1)**.

```bash
command > output.txt 2>&1    # Both stdout and stderr to file
# Modern equivalent:
command &> output.txt
```

</details>

---

**Q17:** What does `{1..5}` expand to?
<details>
<summary>Answer</summary>

**Brace expansion:** `1 2 3 4 5`

More examples:
- `{a..e}` → `a b c d e`
- `file{1,2,3}.txt` → `file1.txt file2.txt file3.txt`
- `{01..10}` → `01 02 03 04 05 06 07 08 09 10`

</details>

---

## Topic 5: Shell Variables

**Q18:** What is the difference between a local variable and an environment variable?
<details>
<summary>Answer</summary>

- **Local variable:** Only visible in current shell
  ```bash
  myvar="hello"
  ```

- **Environment variable:** Exported to child processes
  ```bash
  export myvar="hello"
  ```

</details>

---

**Q19:** What does `${var:-default}` do?
<details>
<summary>Answer</summary>

Uses **default value** if variable is unset or null.

```bash
echo ${NAME:-"Guest"}    # Prints "Guest" if NAME is unset
```

Related:
- `${var:=default}` - Assign default if unset
- `${var:+alt}` - Use alt if set
- `${var:?error}` - Show error if unset

</details>

---

**Q20:** How do you get the length of a string in a variable?
<details>
<summary>Answer</summary>

```bash
${#variable}
```

Example:
```bash
text="Hello World"
echo ${#text}    # 11
```

</details>

---

**Q21:** What does `${path##*/}` extract from a path?
<details>
<summary>Answer</summary>

The **basename** (filename) - removes everything up to and including the last `/`.

```bash
path="/home/user/file.txt"
echo ${path##*/}    # file.txt
```

Related:
- `${path%/*}` → dirname (`/home/user`)

</details>

---

## Topic 6: Shell Environment

**Q22:** What is the difference between ~/.bash_profile and ~/.bashrc?
<details>
<summary>Answer</summary>

- **~/.bash_profile:** Read by **login shells** (SSH, console login)
- **~/.bashrc:** Read by **interactive non-login shells** (GUI terminal)

Best practice: Have `~/.bash_profile` source `~/.bashrc`, put all customizations in `~/.bashrc`.

</details>

---

**Q23:** How do you create a permanent alias?
<details>
<summary>Answer</summary>

Add it to `~/.bashrc`:
```bash
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```

</details>

---

**Q24:** What is wrong with this: `alias greet='echo Hello $1'`?
<details>
<summary>Answer</summary>

**Aliases cannot accept arguments.** `$1` won't work in an alias.

Use a **function** instead:
```bash
greet() {
    echo "Hello $1"
}
greet World    # Hello World
```

</details>

---

**Q25:** What PS1 escape sequence shows the current working directory?
<details>
<summary>Answer</summary>

- `\w` - Full path (e.g., `/home/user/documents`)
- `\W` - Basename only (e.g., `documents`)

Example prompt:
```bash
export PS1="\u@\h:\w\$ "
# user@hostname:/current/path$
```

</details>

---

## Topic 7: Getting Command Information

**Q26:** What command shows the manual page for a command?
<details>
<summary>Answer</summary>

```bash
man command
```

Example:
```bash
man ls
man grep
```

Navigation: Space (next page), `/pattern` (search), `q` (quit)

</details>

---

**Q27:** What is `apropos` used for?
<details>
<summary>Answer</summary>

**Search man pages by keyword.**

```bash
apropos compress    # Find compression-related commands
apropos network     # Find networking tools
# Same as:
man -k keyword
```

</details>

---

**Q28:** When would you use `help` instead of `man`?
<details>
<summary>Answer</summary>

For **shell built-in commands** that don't have man pages.

```bash
help cd       # Works (cd is a builtin)
man cd        # Might not exist

help export
help alias
help history
```

</details>

---

**Q29:** What command shows a one-line description of a command?
<details>
<summary>Answer</summary>

```bash
whatis command
```

Example:
```bash
whatis ls
# ls (1) - list directory contents
```

</details>

---

**Q30:** How do you see which man page section a command belongs to?
<details>
<summary>Answer</summary>

```bash
whatis command
# or
man -k command
```

Sections:
- 1 - User commands
- 5 - File formats
- 8 - Admin commands

Access specific section:
```bash
man 1 printf    # printf command
man 3 printf    # printf() function
```

</details>

---

---

## Topic 8: Advanced & Book Specifics

**Q31:** What does the `fc` command do?
<details>
<summary>Answer</summary>

**Fix Command:** It opens your previous command(s) in a text editor (like vi) so you can edit and run them.

```bash
fc          # Edit last command
fc 100 105  # Edit commands 100-105
```

</details>

---

**Q32:** What is the difference between `find` and `locate`?
<details>
<summary>Answer</summary>

- **find:** Searches the actual filesystem in real-time (slower, but current).
- **locate:** Searches a pre-built database of files (faster, but may be outdated).

If `locate` misses a file, run `sudo updatedb` to update the database.

</details>

---

**Q33:** What does `type -a` show?
<details>
<summary>Answer</summary>

It shows **all locations** containing the command (aliases, built-ins, and file paths).

```bash
type -a ls
# ls is aliased to `ls --color=auto'
# ls is /usr/bin/ls
```

</details>

---

**Q34:** Which environment variable can automatically log you out after a period of inactivity?
<details>
<summary>Answer</summary>

**TMOUT**

Example: `export TMOUT=1800` log out after 30 minutes (1800 seconds) of no input.

</details>

---

## Bonus: True/False

**Q35-39:** True or False?

<details>
<summary>Questions</summary>

35. Aliases can accept command-line arguments.
36. `~` expands to the current user's home directory.
37. `Ctrl+R` searches forward through command history.
38. Environment variables are inherited by child processes.
39. The `/etc/profile` file is read for every new terminal window.

</details>

<details>
<summary>Answers</summary>

35. **FALSE** - Use functions for arguments
36. **TRUE** - `~` = `$HOME`
37. **FALSE** - `Ctrl+R` searches **backward**; `Ctrl+S` searches forward
38. **TRUE** - That's what `export` enables
39. **FALSE** - Only login shells read `/etc/profile`

</details>

---

## Command Quick Review

**Q40:** Match the command/shortcut to its purpose:

| Item | Purpose |
|------|---------|
| `!!` | ? |
| `$?` | ? |
| `Ctrl+Z` | ? |
| `&>` | ? |
| `type` | ? |
| `fc` | ? |

<details>
<summary>Answer</summary>

| Item | Purpose |
|------|---------|
| `!!` | Repeat last command |
| `$?` | Exit status of last command |
| `Ctrl+Z` | Suspend current process |
| `&>` | Redirect stdout and stderr |
| `type` | Show how command is interpreted |
| `fc` | Edit previous command aliases in editor |

</details>

---

## Study Tips

1. **Practice in terminal** - Try every command and shortcut
2. **Customize your shell** - Create aliases and functions you'll use
3. **Read man pages** - `man bash` is comprehensive
4. **Break complex commands** - Use `set -x` to trace execution

---

*Quiz based on Linux Bible 10th Edition - Part 2: Becoming a Linux Power User*
