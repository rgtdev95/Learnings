# Password Security

## Common Weak Passwords

**Most popular (and worst) passwords:**
*   123456
*   Password
*   princess
*   rockyou
*   abc123

**Why they're dangerous:**
*   Easy to guess
*   In attacker dictionaries
*   Brute force targets
*   Publicly available lists

**Find password lists:**
*   Search "common passwords"
*   If you can find them, attackers can too

---

## Choosing Good Passwords

### What NOT to Do

**Avoid these:**
*   Login name or variations
*   Full name
*   Dictionary words
*   Proper names
*   Phone numbers
*   Addresses
*   Family or pet names
*   Website names
*   Keyboard patterns (qwerty, asdfg)
*   Above with numbers/punctuation added
*   Above typed backward

### What TO Do

**Strong password requirements:**

**1. Length: 15-25 characters**
*   Longer = more secure
*   Minimum depends on security needs
*   25 characters recommended

**2. Character variety:**
*   Lowercase letters (a-z)
*   Uppercase letters (A-Z)
*   Numbers (0-9)
*   Special characters (! @ # $ % * ( ) - + = , < > : ; " ')

---

## Creating Memorable Strong Passwords

**Method: First letter of sentence**

**How it works:**
1. Choose meaningful sentence (only to you)
2. Take first letter of each word
3. Add numbers and special characters
4. Vary case

**Examples:**

| Password | How to Remember |
|----------|-----------------|
| Mrci7yo! | My rusty car is 7 years old! |
| 2emBp1ib | 2 elephants make BAD pets, 1 is better |
| ItMc?Gib | Is that MY coat? Give it back |

**Important:**
*   Sentence must be private
*   Not publicly available
*   Meaningful only to you
*   Don't use examples shown here!

---

## Setting and Changing Passwords

**Change your password:**
```bash
passwd
```

**Process:**
1. Enter old password (not displayed)
2. Enter new password
3. Checked by cracklib
4. Re-enter new password (verify)

**Change another user's password (root only):**
```bash
passwd username
```

**Example:**
```bash
# passwd joe
Changing password for user joe.
New UNIX password: ********
Retype new UNIX password: ********
passwd: all authentication tokens updated successfully.
```

**Note:**
*   Root doesn't need old password
*   Root can assign bad passwords
*   Non-root users must use strong passwords

---

## Enforcing Password Requirements with PAM

**Configure password requirements:**

**Edit PAM configuration:**
```bash
# Fedora/RHEL
vi /etc/pam.d/system-auth

# Ubuntu/Debian
vi /etc/pam.d/common-password
```

**Example PAM rule:**
```
password requisite pam_cracklib.so minlen=12 dcredit=2 ucredit=3 lcredit=2 difok=4
```

**Parameters:**
*   `minlen=12` - Minimum 12 characters
*   `dcredit=2` - At least 2 digits
*   `ucredit=3` - At least 3 uppercase
*   `lcredit=2` - At least 2 lowercase
*   `difok=4` - Different from previous passwords

---

## Password Aging Settings

### /etc/login.defs (New Accounts)

**Default settings for new accounts:**
```bash
PASS_MAX_DAYS   30    # Max days before change required
PASS_MIN_DAYS   5     # Min days before can change again
PASS_MIN_LEN    16    # Minimum password length
PASS_WARN_AGE   7     # Days of warning before expiration
```

**PASS_MAX_DAYS:**
*   How often password must change
*   30 days for shared accounts
*   Can be longer for individual accounts

**PASS_MIN_DAYS:**
*   Prevents immediate password change back
*   Stops users from cycling passwords
*   Set to 5+ days

**PASS_WARN_AGE:**
*   Warning period before forced change
*   7 days recommended
*   Users need reminders

**PASS_MIN_LEN:**
*   Minimum password length
*   15-25 characters recommended
*   Based on security needs

**Note:**
*   Ubuntu uses PAM for PASS_MIN_LEN
*   Not in login.defs on Ubuntu

---

## Password Aging with chage

**For existing accounts:**

**chage options:**

| Option | Description | Equivalent Setting |
|--------|-------------|-------------------|
| -M | Max days before change | PASS_MAX_DAYS |
| -m | Min days before can change | PASS_MIN_DAYS |
| -W | Warning days | PASS_WARN_AGE |

**View password aging:**
```bash
chage -l username | grep days
```

**Example output:**
```
Minimum number of days between password change        : 0
Maximum number of days between password change        : 99999
Number of days of warning before password expires     : 7
```

**Set password aging:**
```bash
# Set all at once
chage -M 30 -m 5 -W 7 username

# Verify
chage -l username | grep days
```

**Example output after change:**
```
Minimum number of days between password change        : 5
Maximum number of days between password change        : 30
Number of days of warning before password expires     : 7
```

---

## Account Locking via Password Expiration

**Lock account after password expires:**

**View password expiration:**
```bash
chage -l username | grep Password
```

**Example output:**
```
Password expires                                      : never
Password inactive                                     : never
```

**Set password expiration:**
```bash
# -M: Max days before password expires
# -I: Days after expiration before account locks
chage -M 30 -I 5 username
```

**Verify:**
```bash
chage -l username | grep Password
```

**Example output:**
```
Password expires                                      : Mar 03, 2021
Password inactive                                     : Mar 08, 2021
```

**Result:**
*   Password expires in 30 days
*   Account locks 5 days after password expires
*   Useful for departed employees

**Important:**
*   `-I` option requires `-M` to be set
*   Without password expiration, `-I` has no effect

---

## Password Files and Hashes

### /etc/passwd (Old Method)

**Historical storage:**
*   Passwords stored here originally
*   Hashed, but readable by all
*   Security risk

**File permissions:**
```bash
ls -l /etc/passwd
-rw-r--r--. 1 root root 1644 Feb  2 02:30 /etc/passwd
```

**Problem:**
*   Everyone can read file
*   Rainbow tables can crack hashes
*   Hashes + salts exposed

**Example hash:**
```
$6$dhN5ZMUj$CNghjYIteau5xl8yX.f6PTOpendJwTOcXjlTDQUQZhhy
V8hKzQ6Hxx6Egj8P3VsHJ8Qrkv.VSR5dxcK3QhyMc.
```

### /etc/shadow (Current Method)

**Modern storage:**
*   Hashed passwords moved here
*   Only root can read
*   Much more secure

**File permissions:**
```bash
ls -l /etc/shadow
----------. 1 root root 1049 Feb  2 09:45 /etc/shadow
```

**Example entries:**
```
johndoe:$6$jJjdRN9/qELmb8xWM1LgOYGhEIxc/:15364:0:99999:7:::
Tim:$6$z760AJ42$QXdhFyndpbVPVM5oVtNHs4B/:15372:5:30:7:16436::
```

**Fields in /etc/shadow:**
1. Username
2. Hashed password
3. Days since 1/1/1970 password changed
4. Min days before can change
5. Max days before must change
6. Warning days
7. Inactive days after expiration
8. Account expiration date (days since 1/1/1970)
9. Reserved

**Convert old system:**
```bash
# Create /etc/shadow and move passwords
pwconv
```

---

## Password Generation Tools

**pwgen utility:**

**Install:**
```bash
dnf install pwgen
apt-get install pwgen
```

**Features:**
*   Generates pronounceable passwords
*   Memorable yet secure
*   Good starting point

**Usage:**
```bash
# Generate passwords
pwgen

# Specific length
pwgen 16

# Secure passwords
pwgen -s 20
```

---

## Review Questions

1. What are two requirements for a strong password?

2. How do you change your own password?

3. What file contains default password aging settings for new accounts?

4. What command sets password aging for existing accounts?

5. Where are hashed passwords stored on modern Linux systems?

6. What command converts old /etc/passwd passwords to /etc/shadow?

7. True or False: Root users can assign weak passwords.

8. What does the -I option in chage do?

---

## Quick Reference

```
Strong Password Rules:
├── Length: 15-25 characters
├── Lowercase letters
├── Uppercase letters
├── Numbers
└── Special characters

Avoid:
├── Dictionary words
├── Names (yours, pets, family)
├── Phone numbers, addresses
├── Keyboard patterns (qwerty)
└── Common passwords (123456, Password)

Change Password:
├── passwd              → Change your password
└── passwd username     → Change user's password (root)

Password Aging (/etc/login.defs):
├── PASS_MAX_DAYS   → Max days before change
├── PASS_MIN_DAYS   → Min days before can change
├── PASS_MIN_LEN    → Minimum length
└── PASS_WARN_AGE   → Warning days

chage Command:
├── chage -l user                → View aging info
├── chage -M 30 user             → Max 30 days
├── chage -m 5 user              → Min 5 days
├── chage -W 7 user              → Warn 7 days
├── chage -M 30 -m 5 -W 7 user   → Set all
└── chage -M 30 -I 5 user        → Lock after expiration

Password Files:
├── /etc/passwd   → User accounts (readable by all)
├── /etc/shadow   → Hashed passwords (root only)
├── /etc/group    → Groups
└── /etc/gshadow  → Group passwords

File Permissions:
├── /etc/passwd  → -rw-r--r-- (644)
├── /etc/shadow  → ---------- (000)
├── /etc/group   → -rw-r--r-- (644)
└── /etc/gshadow → ---------- (000)

PAM Configuration:
├── Fedora/RHEL: /etc/pam.d/system-auth
├── Ubuntu: /etc/pam.d/common-password
└── Example: password requisite pam_cracklib.so minlen=12

Password Generation:
└── pwgen → Generate pronounceable passwords

Convert to Shadow:
└── pwconv → Move passwords to /etc/shadow
```
