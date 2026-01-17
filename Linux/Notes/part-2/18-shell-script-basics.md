# Shell Script Basics

## Key Concepts

| Term | Definition |
|------|------------|
| **Shell Script** | A text file containing a sequence of commands to run. |
| **Shebang (`#!`)** | The first line telling Linux which interpreter to use. |
| **Variables** | Named placeholders for storing data (strings/numbers). |
| **Parameters** | Arguments passed to the script at runtime (`$1`, `$2`). |

---

## Creating a Script

1.  **Create File:** `vim myscript.sh`
2.  **Add Shebang:** `#!/bin/bash`
3.  **Add Commands:** `echo "Hello World"`
4.  **Make Executable:** `chmod +x myscript.sh`
5.  **Run:** `./myscript.sh`

---

## Variables

### Defining Variables
*   No spaces around types!
*   **Good:** `NAME="Joe"`
*   **Bad:** `NAME = "Joe"`

### Using Variables
Use `$` to reference a variable.
```bash
NAME="Christopher"
echo "Hello, $NAME"
```

### Expanding Commands
Store the output of a command in a variable using `$()` or backticks `` ` ` ``.
```bash
TODAY=$(date)
FILES=`ls | wc -l`
echo "Today is $TODAY and you have $FILES files."
```

---

## Positional Parameters (Arguments)

Special variables for handling arguments passed to the script: `myscript.sh arg1 arg2`

| Variable | Meaning |
|----------|---------|
| `$0` | Name of the script itself |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Total number of arguments |
| `$@` | List of all arguments |
| `$?` | Exit status of last command (0 = Success) |

**Example:**
```bash
#!/bin/bash
echo "Script Name: $0"
echo "Hello, $1!"
```
*Run:* `./myscript.sh Alice` -> *Output:* `Hello, Alice!`

---

## User Input (`read`)

Ask the user for input during execution.

```bash
echo "What is your name?"
read NAME
echo "Nice to meet you, $NAME!"
```

**One-line prompt:**
```bash
read -p "Enter your age: " AGE
```

---

## Review Questions

1. What specific characters must appear at the very start of a shell script?

2. Which command makes a script executable?

3. How do you assign the string "Linux" to a variable named `OS`?

4. How do you print the value of the `OS` variable?

5. What variable holds the **first** argument passed to a script?

6. What does `$#` represent?

7. Which command captures user input into a variable?

8. If a script runs successfully, what is the value of `$?`?

---

## Quick Reference

```
Basics:
├── #!/bin/bash          → Shebang
├── chmod +x script.sh   → Make runnable
└── ./script.sh          → Run

Variables:
├── VAR="value"          → Set (no spaces)
├── echo $VAR            → Use
└── RES=$(cmd)           → Store command output
```
