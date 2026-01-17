# Loops and Text Filters

## Key Concepts

| Term | Definition |
|------|------------|
| **for loop** | Repeat action for a specific list of items. |
| **while loop** | Repeat action WHILE a condition is true. |
| **until loop** | Repeat action UNTIL a condition becomes true. |
| **Text Filters** | Tools to process streams of text (`cut`, `sed`, `tr`, `awk`). |

---

## Loops

### `for` Loop
Iterate over a list (files, numbers, strings).

```bash
# Loop through simple numbers
for i in 1 2 3; do
    echo "Number: $i"
done

# Loop through files in output
for FILE in $(ls *.log); do
    echo "Found log: $FILE"
done
```

### `while` Loop
Run as long as the test is true.

```bash
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count is $COUNT"
    let COUNT=COUNT+1
done
```

---

## Useful Text Filters

These tools are often used in scripts to clean up data.

### 1. `cut`
Extract columns (fields) from text.
*   `-d`: Delimiter (separator)
*   `-f`: Field number

```bash
# Get 1st column (username) from /etc/passwd (separated by :)
cut -d ":" -f 1 /etc/passwd
```

### 2. `tr` (Translate)
Replace or delete characters.

```bash
# Lowercase to Uppercase
echo "hello" | tr a-z A-Z

# Replace spaces with underscores
echo "My File Name" | tr " " "_"
```

### 3. `sed` (Stream Editor)
Search and replace text automatically.
*   `s/find/replace/g`

```bash
# Replace 'apples' with 'oranges'
echo "I like apples" | sed 's/apples/oranges/g'
```

### 4. `grep` (Review)
Filter lines matching a pattern.

```bash
# Only show lines with "Error"
cat log.txt | grep "Error"
```

---

## Review Questions

1. Which loop type is best for iterating over a known list of files?

2. Which command would you use to extract just the 3rd column of a CSV file?

3. How do you translate all lowercase letters to uppercase in a pipe?

4. What `sed` command replaces "foo" with "bar"?

5. Which loop continues running as long as a condition is **false** (until it becomes true)?

6. In a `for` loop, what keyword ends the loop block?

7. Write a `cut` command to get the usernames (field 1) from `/etc/passwd`.

8. True or False: `while` loops run as long as the test returns 0 (Success).

---

## Quick Reference

```
Loops:
for VAR in LIST; do ... done
while [ cond ]; do ... done

Filters:
cut -d":" -f1     → Extract field 1
tr a-z A-Z        → Translate case
sed 's/old/new/g' → Replace text
```
