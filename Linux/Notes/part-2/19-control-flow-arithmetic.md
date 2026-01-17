# Control Flow and Arithmetic

## Key Concepts

| structure | Purpose |
|-----------|---------|
| **if-then-else** | Conditional execution (do this IF true) |
| **test (`[...]`)** | Evaluates conditions (file exists? string equal?) |
| **case** | Clean way to handle multiple choices (like switch) |
| **Arithmetic** | Performing math (`let`, `expr`, `$((...))`) |

---

## The `if` Statement

Checks the exit status of a command/test. If 0 (Success/True), it runs the `then` block.

```bash
if [ "$1" = "Hello" ]; then
    echo "Well, hello to you too!"
else
    echo "You didn't say Hello back :("
fi
```

### Common Test Operators

**Inside `[ ... ]`:**

| Type | Operator | Meaning |
|------|----------|---------|
| **String** | `=` | Equal strictly |
| | `!=` | Not Equal |
| | `-z` | String is empty (Zero length) |
| **File** | `-f` | File exists |
| | `-d` | Directory exists |
| | `-e` | Exists (any type) |
| **Integer** | `-eq` | Equal |
| | `-gt` | Greater Than |
| | `-lt` | Less Than |

**Example:**
```bash
if [ -f "/etc/passwd" ]; then
    echo "Password file exists."
fi
```

---

## The `case` Statement

Good for menus or checking specific string values.

```bash
case "$1" in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    *)
        echo "Usage: $0 {start|stop}"
        ;;
esac
```
*   `*)` acts as the "default" or "catch-all" option.
*   `;;` ends each block.

---

## Arithmetic

Bash variables are strings by default. Use these tools for math:

1.  **`let` command:**
    ```bash
    let RESULT=10+5
    ```

2.  **Double Parentheses `$((...))`:** (Preferred/Modern)
    ```bash
    NUM=5
    echo $((NUM + 10))
    ```

3.  **`expr` command:** (Old school, requires spaces)
    ```bash
    expr 5 + 5
    ```

4.  **`bc` command:** (For floating point/decimals)
    ```bash
    echo "10.5 / 2" | bc
    ```

---

## Review Questions

1. Which closing word ends an `if` statement?

2. What comparison operator checks if two numbers are equal?

3. What comparison operator checks if a file exists?

4. How do you end a block in a `case` statement?

5. Write a command to add 5 and 10 using double parentheses.

6. If `[ -z "$VAR" ]` is true, what do we know about `$VAR`?

7. What does the `*)` pattern match in a `case` statement?

8. True or False: You should put spaces around the standard assignment operator (`VAR=5`). (False!)

---

## Quick Reference

```
if:
if [ condition ]; then
  ...
elif [ condition ]; then
  ...
else
  ...
fi

math:
VAL=$(( 5 + 5 ))
```
