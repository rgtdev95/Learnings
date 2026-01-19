# GPG Encryption and File Security

## GNU Privacy Guard (GPG)

**What is GPG:**
*   OpenPGP utility
*   Command: `gpg2`
*   Installed by default (most distros)
*   Versatile cryptography tool

**Can use:**
*   Symmetric encryption (single key)
*   Asymmetric encryption (key pairs)
*   Digital signatures

**Installation:**
```bash
# Fedora/RHEL (usually pre-installed)
dnf install gnupg2

# Ubuntu
apt-get install gnupg2
```

---

## Symmetric Encryption with GPG

### Encrypting a File

**Create encrypted file:**
```bash
# Encrypt with symmetric key
gpg2 -c --force-mdc -o backup.tar.gz.gpg backup.tar.gz
```

**Options:**
*   `-c` - Use symmetric cipher
*   `--force-mdc` - Force modification detection code
*   `-o filename` - Output file

**Process:**
1. Enter passphrase (protects key)
2. Repeat passphrase
3. Encrypted file created

**Example:**
```bash
# Create tar archive
tar -czf /tmp/backup.tar.gz /etc

# Encrypt it
gpg2 -c --force-mdc -o /tmp/backup.tar.gz.gpg /tmp/backup.tar.gz

# Verify encryption
file /tmp/backup.tar.gz.gpg
# Output: GPG symmetrically encrypted data (CAST5 cipher)
```

### Decrypting a File

**Decrypt file:**
```bash
gpg2 -d --force-mdc /tmp/backup.tar.gz.gpg > /tmp/backup.tar.gz
```

**Options:**
*   `-d` - Decrypt

**Process:**
1. Prompted for passphrase (pop-up window)
2. File decrypted
3. Output to specified file

**Note:**
*   If `gpg-agent` running, passphrase cached
*   Won't need to re-enter for subsequent decryptions

---

## Asymmetric Encryption with GPG

### Generating a Key Pair

**Create key pair:**
```bash
gpg2 --gen-key
```

**Interactive prompts:**

**1. Real name:**
```
Real name: John Doe
```

**2. Email address:**
```
Email address: jdoe@example.com
```

**3. Confirm USER-ID:**
```
You selected this USER-ID:
    "John Doe <jdoe@example.com>"
Change (N)ame, (E)mail or (O)kay/(Q)uit? O
```

**4. Passphrase:**
*   Protects private key
*   Pop-up window prompts
*   Enter twice

**Output:**
```
gpg: key 383D645D9798C173 marked as ultimately trusted
public and secret key created and signed.

pub   rsa2048 2019-10-27 [SC] [expires: 2021-10-26]
      7469BCD3D05A43130F1786E0383D645D9798C173
uid   John Doe <jdoe@example.com>
sub   rsa2048 2019-10-27 [E] [expires: 2021-10-26]
```

**Key components:**
*   **User ID:** Identifies public key
*   **Email:** Associated with key
*   **Passphrase:** Protects private key
*   **Key ring:** Storage for keys

---

### Viewing Keys

**List public keys:**
```bash
gpg2 --list-keys
```

**Example output:**
```
/home/jdoe/.gnupg/pubring.kbx
-----------------------------
pub   rsa2048 2019-10-27 [SC] [expires: 2021-10-26]
      7469BCD3D05A43130F1786E0383D645D9798C173
uid   [ultimate] John Doe <jdoe@example.com>
sub   rsa2048 2019-10-27 [E] [expires: 2021-10-26]
```

**List private keys:**
```bash
gpg2 --list-secret-keys
```

---

### Exporting Public Key

**Extract public key:**
```bash
gpg2 --export "John Doe" > JohnDoe.pub
```

**Verify export:**
```bash
file JohnDoe.pub
# Output: PGP/GPG key public ring (v4) created Sun Oct 27...
```

**Purpose:**
*   Share with others
*   They can encrypt files for you
*   Public key is safe to share

---

### Importing Public Key

**Add someone's public key:**
```bash
gpg2 --import JohnDoe.pub
```

**Output:**
```
gpg: key 383D645D9798C173: public key "John Doe <jdoe@example.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

**Verify import:**
```bash
gpg2 --list-keys
```

---

### Encrypting with Public Key

**Encrypt file for recipient:**
```bash
gpg2 --out MessageForJohn --recipient "John Doe" --encrypt MessageForJohn.txt
```

**Options:**
*   `--out filename` - Output encrypted file
*   `--recipient "Name"` - Use this person's public key
*   `--encrypt` - Encrypt the file

**Result:**
*   Plain text file encrypted
*   Only recipient's private key can decrypt
*   Secure transmission

---

### Decrypting with Private Key

**Decrypt received file:**
```bash
gpg2 --out JillsMessage --decrypt MessageForJohn
```

**Process:**
1. Prompted for passphrase (protects private key)
2. File decrypted with private key
3. Output to specified file

**Example output:**
```
gpg: encrypted with 2048-bit RSA key, ID D9EBC5F7317D3830, created 2019-10-27
      "John Doe <jdoe@example.com>"
```

**Read decrypted message:**
```bash
cat JillsMessage
```

---

## Asymmetric Encryption Process Summary

**Complete workflow:**

**1. Generate key pair:**
```bash
gpg2 --gen-key
```

**2. Export public key:**
```bash
gpg2 --export "Your Name" > YourName.pub
```

**3. Share public key:**
*   Email attachment
*   Web page
*   File transfer

**4. Recipient imports your public key:**
```bash
gpg2 --import YourName.pub
```

**5. Recipient encrypts file with your public key:**
```bash
gpg2 --out encrypted_file --recipient "Your Name" --encrypt plain_file
```

**6. Recipient sends encrypted file to you**

**7. You decrypt with your private key:**
```bash
gpg2 --out decrypted_file --decrypt encrypted_file
```

---

## File Encryption Tools

**gpg2:**
*   Most popular
*   Symmetric or asymmetric
*   Installed by default
*   Configuration: `/etc/gnupg/gpg.conf`

**aescrypt:**
*   Uses AES (Rijndael) cipher
*   Symmetric encryption
*   Third-party FOSS
*   Website: www.aescrypt.com

**bcrypt:**
*   Uses Blowfish cipher
*   Symmetric encryption
*   Not installed by default

**Install:**
```bash
# Fedora
yum install bcrypt

# Ubuntu
apt-get install bcrypt
```

**ccrypt:**
*   Uses AES (Rijndael) cipher
*   Replaces Unix crypt utility
*   Not installed by default

**Install:**
```bash
# Fedora
yum install ccrypt

# Ubuntu
apt-get install ccrypt
```

---

## Miscellaneous Cryptography Tools

**Duplicity:**
*   Encrypts backups
*   Network backup solution

**Install:**
```bash
# Fedora
yum install duplicity

# Ubuntu
apt-get install duplicity
```

**gpg-zip:**
*   Encrypts/signs files into archive
*   Uses GNU Privacy Guard
*   Installed by default

**OpenSSL:**
*   SSL/TLS toolkit
*   Implements secure protocols
*   Installed by default

**Seahorse:**
*   GPG encryption key manager
*   GUI application
*   Installed by default (Ubuntu)

**Install (Fedora/RHEL):**
```bash
yum install seahorse
```

**SSH:**
*   Encrypts remote access
*   Network encryption
*   Installed by default

**zipcrypt:**
*   Encrypts Zip file entries
*   Installed by default

---

## Desktop Encryption Management

**Passwords and Keys (Seahorse):**

**Launch:**
```bash
seahorse
```

**Or:** Activities → Passwords and Keys

**Features:**

**Passwords:**
*   Saved website passwords
*   Chrome/Chromium credentials
*   Login entry storage

**Certificates:**
*   Gnome2 Key Storage
*   User Key Storage
*   System Trust
*   Default Trust

**PGP keys:**
*   View GPG keys
*   GnuPG keys entry
*   Manage key pairs

**Secure Shell:**
*   OpenSSH keys
*   Public/private keys
*   Passwordless authentication

---

## Review Questions

1. What command generates a GPG key pair?

2. How do you encrypt a file with symmetric encryption using GPG?

3. What's the difference between symmetric and asymmetric encryption in GPG?

4. How do you export your public key?

5. What command imports someone else's public key?

6. What protects your private key?

7. True or False: You can share your private key with others.

8. What tool provides a GUI for managing GPG keys?

---

## Quick Reference

```
GPG Commands:
├── gpg2 --gen-key                    → Generate key pair
├── gpg2 --list-keys                  → List public keys
├── gpg2 --list-secret-keys           → List private keys
├── gpg2 --export "Name" > file.pub   → Export public key
├── gpg2 --import file.pub            → Import public key
└── gpg2 --delete-key "Name"          → Delete key

Symmetric Encryption:
├── Encrypt: gpg2 -c -o encrypted.gpg plainfile
└── Decrypt: gpg2 -d encrypted.gpg > plainfile

Asymmetric Encryption:
├── Encrypt: gpg2 --out encrypted --recipient "Name" --encrypt plainfile
└── Decrypt: gpg2 --out plainfile --decrypt encrypted

Key Pair Generation:
1. gpg2 --gen-key
2. Enter real name
3. Enter email
4. Enter passphrase (twice)
5. Keys generated

Public Key Sharing:
1. gpg2 --export "Name" > name.pub
2. Share name.pub file
3. Recipient: gpg2 --import name.pub
4. Recipient encrypts with your public key
5. You decrypt with your private key

File Encryption Tools:
├── gpg2      → Default, versatile
├── aescrypt  → AES cipher
├── bcrypt    → Blowfish cipher
└── ccrypt    → AES cipher

Misc Tools:
├── duplicity → Backup encryption
├── gpg-zip   → Archive encryption
├── openssl   → SSL/TLS toolkit
├── seahorse  → GUI key manager
├── ssh       → Network encryption
└── zipcrypt  → Zip encryption

Seahorse (GUI):
├── Launch: seahorse
├── Passwords → Website credentials
├── Certificates → Trust stores
├── PGP keys → GPG keys
└── Secure Shell → SSH keys

Key Storage:
├── Public keys: ~/.gnupg/pubring.kbx
├── Private keys: ~/.gnupg/secring.gpg
└── Trust DB: ~/.gnupg/trustdb.gpg

Best Practices:
├── Never share private key
├── Protect passphrase
├── Backup private key securely
├── Use strong passphrases
├── Verify key fingerprints
└── Revoke compromised keys
```
