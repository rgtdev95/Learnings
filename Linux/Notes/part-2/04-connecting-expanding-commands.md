# Connecting and Expanding Commands

## Key Concepts

| Term | Definition |
|------|------------|
| **Pipe (`\|`)** | Connecting the stdout of one command to the stdin of another |
| **Metacharacters** | Special characters like `|`, `&`, `;`, `(` `)`, `<`, `>` |
| **Command Substitution** | Using the output of a command as an argument for another |
| **Expansion** | Shell interpreting arithmetic `$[ ]`, variables `$VAR`, etc. |

---

## Piping Commands

The pipe metacharacter (`|`) connects commands. It takes the output of the first command and feeds it as input to the next command.

### Book Example: Sorting Passwd

```bash
cat /etc/passwd | sort | less
```
1. `cat` lists the password file
2. `sort` puts lines in alphabetical order
3. `less` displays the result page by page

### Book Example: Formatted Man Page

```bash
gunzip < /usr/share/man/man1/grep.1.gz | nroff -c -man | less
```
Demonstrates the "building block" philosophy of Linux:
- Unzip file → Format text → Display text

---

## Sequential Commands

Run multiple commands on one line by separating them with semicolons (`;`).

```bash
date ; troff -me verylargedocument | lpr ; date
```
This example measures how long a formatting job takes:
1. Show start time (`date`)
2. Format and print document (`troff` piped to `lpr`)
3. Show end time (`date`)

> **Tip:** Add `mail` to the end to get notified when a long job finishes:
> `; mail -s "Job Done" user@example.com`

---

## Background Commands

Use the ampersand (`&`) to run commands in the background so you don't tie up your shell.

```bash
troff -me verylargedocument | lpr &
```
The shell returns instantly, and the command runs in the background.

---

## Expanding Commands (Substitution)

Use the output of a command as an argument.

**Syntax:** `$(command)` or `` `command` ``

### Example: Editing Found Files
```bash
vi $(find /home | grep xyzzy)
```
1. `find` lists all files in `/home`
2. `grep` filters for filenames containing "xyzzy"
3. `vi` opens those specific files for editing

---

## Expanding Arithmetic Expressions

The shell can do math for you.

**Syntax:** `$[expression]` or `$(expression)`

```bash
echo "I am $[2020 - 1957] years old."
# Output: I am 63 years old.
```

**Using with commands:**
```bash
echo "There are $(ls | wc -w) files in this directory."
# Output: There are 14 files in this directory.
```

---

## Expanding Variables

Use the dollar sign (`$`) to expand variables.

```bash
ls -l $BASH
# Lists detailed info for /bin/bash
```

If you define a variable:
```bash
M=/work/time/files/info/memos ; export M
cd $M
vi $M/bun
```
This creates a shortcut to a long directory path.

---

## Review Questions

1. What metacharacter is used for piping?

2. How do you run two commands sequentially on one line?

3. What character runs a command in the background?

4. What is the preferred syntax for command substitution?

5. How would you calculate `2020 - 1980` and echo the result using shell expansion?

6. Rewrite this command using command substitution: `date`
   `echo "Today is ..."`

7. What does the variable `$BASH` contain?

8. Why is piping considered a "building block" feature?

---

## Quick Reference

```
Connecting:
├── cmd1 | cmd2         → Pipe output to input
├── cmd1 ; cmd2         → Run sequentially
└── cmd1 &              → Run in background

Expansion:
├── $(cmd)              → Command substitution
├── `cmd`               → Command substitution (legacy)
├── $[1+2]              → Arithmetic expansion
└── $VAR                → Variable expansion
```
