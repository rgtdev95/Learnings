# Cryptography Basics

## Cryptography Terms

**Plain text:**
*   Text readable by humans or machines
*   Unencrypted data
*   Original message

**Ciphertext:**
*   Text unreadable by humans or machines
*   Encrypted data
*   Scrambled message

**Encryption:**
*   Process of converting plain text to ciphertext
*   Uses an algorithm (cipher)
*   Requires a key

**Decryption:**
*   Process of converting ciphertext to plain text
*   Uses an algorithm (cipher)
*   Requires a key

**Cipher:**
*   Algorithm used for encryption/decryption
*   Mathematical process
*   Examples: AES, RSA, Blowfish

**Block cipher:**
*   Breaks data into blocks before encrypting
*   Fixed-size chunks
*   Examples: AES, DES, Blowfish

**Stream cipher:**
*   Encrypts data without breaking it up
*   Continuous flow
*   Example: RC4

**Key:**
*   Data required by cipher
*   Needed to encrypt/decrypt
*   Size affects security

---

## Hashing vs Encryption

### Hashing

**What it is:**
*   One-way mathematical process
*   Creates ciphertext (hash)
*   Cannot be reversed
*   Collision-free (unique outputs)

**Not encryption:**
*   Cannot de-hash back to plain text
*   Different purpose than encryption
*   Used for verification, not secrecy

**Uses on Linux:**
*   Passwords (/etc/shadow)
*   File verification
*   Digital signatures
*   Virus signatures

**Also called:**
*   Message digest
*   Checksum
*   Fingerprint
*   Signature

---

## File Integrity with Hashing

### SHA-256 Hash

**Purpose:**
*   Verify file not corrupted
*   Ensure download integrity
*   Detect tampering

**Create hash:**
```bash
sha256sum Fedora-Workstation-Live-x86_64-30-1.2.iso
```

**Example output:**
```
a4e2c49368860887f1cc1166b0613232d4d5de6b46f29c9756bc7cfd5e13f39f
Fedora-Workstation-Live-x86_64-30-1.2.iso
```

**Verify:**
1. Download file
2. Create hash of downloaded file
3. Compare to published hash
4. If match = file not corrupted

---

## SHA-2 Hash Utilities

**Available tools:**
*   `sha224sum` - 224-bit key
*   `sha256sum` - 256-bit key
*   `sha384sum` - 384-bit key
*   `sha512sum` - 512-bit key

**Usage:**
```bash
# Create hash
sha256sum filename

# Verify hash
sha256sum -c hashfile.txt
```

**Key length:**
*   Longer key = more secure
*   Less chance of cracking
*   SHA-512 most secure

**SHA-3:**
*   Released by NIST (August 2015)
*   Newer standard
*   Alternative to SHA-2

---

## Encryption/Decryption Uses

**What can be encrypted:**
*   Individual files
*   Partitions and volumes
*   Web page connections (HTTPS)
*   Network connections (SSH, VPN)
*   Backups
*   Zip files
*   Email

---

## Cryptographic Ciphers

### Symmetric Ciphers

**AES (Advanced Encryption Standard / Rijndael):**
*   Symmetric cryptography
*   Block cipher
*   Block sizes: 128, 192, 256, 512 bits
*   Key sizes: 128, 192, 256, 512 bits
*   Very secure

**Blowfish:**
*   Symmetric cryptography
*   Block cipher
*   64-bit blocks
*   Key: 32-bit to 448-bit
*   Fast and secure

**CAST5:**
*   Symmetric cryptography
*   Block cipher
*   64-bit blocks
*   Key: up to 128-bit

**DES (Data Encryption Standard):**
*   **NO LONGER SECURE**
*   Symmetric cryptography
*   64-bit blocks
*   56-bit key (too small)
*   Easily cracked

**3DES (Triple DES):**
*   Improved DES
*   Symmetric cryptography
*   Encrypts up to 48 times
*   Three different 56-bit keys
*   More secure than DES

**IDEA:**
*   Symmetric cryptography
*   Block cipher
*   64-bit blocks
*   128-bit key

**RC4 (ArcFour/ARC4):**
*   Stream cipher
*   64-bit blocks
*   Variable key size

**RC5:**
*   Symmetric cryptography
*   Block cipher
*   Blocks: 32, 64, or 128 bits
*   Key: up to 2,048 bits

**RC6:**
*   Symmetric cryptography
*   Same as RC5, but faster

### Asymmetric Ciphers

**RSA:**
*   Most popular asymmetric cipher
*   Two keys (public/private pair)
*   Algorithm uses prime numbers
*   Widely used

**El Gamal:**
*   Asymmetric cryptography
*   Two keys from logarithm algorithm

**Elliptic Curve Cryptosystems:**
*   Asymmetric cryptography
*   Two keys from elliptic curve points
*   ECDSA provides better protection than DSA

**Ed25519:**
*   More secure than RSA
*   Faster than RSA
*   Modern choice for signatures

---

## Cipher Keys

**Key size importance:**
*   Bigger key = harder to crack
*   DES (56-bit) = insecure
*   AES (256-bit) = very secure
*   512-bit = extremely secure

**Cracking time:**
*   Small keys: hours/days
*   Large keys: trillions of years

---

## Symmetric Key Cryptography

**Also called:**
*   Secret key cryptography
*   Private key cryptography

**How it works:**
*   Single key for encryption
*   Same key for decryption
*   Must share key securely

**Advantages:**
*   Fast
*   Efficient
*   Simple

**Disadvantages:**
*   Key sharing problem
*   Key management
*   Less secure key distribution

---

## Asymmetric Key Cryptography

**Also called:**
*   Public/private key cryptography

**How it works:**
*   Two keys (key pair)
*   Public key: can be shared
*   Private key: keep secret
*   Encrypt with public key
*   Decrypt with private key

**Advantages:**
*   Heightened security
*   No need to share private key
*   Secure key distribution

**Disadvantages:**
*   Slower than symmetric
*   More complex
*   Key management overhead

**Flexibility:**
*   Can encrypt with private key
*   Decrypt with public key
*   Used for digital signatures

---

## Review Questions

1. What is the difference between plain text and ciphertext?

2. What is hashing and how is it different from encryption?

3. What command creates a SHA-256 hash?

4. What is a block cipher?

5. Why is DES no longer considered secure?

6. What's the difference between symmetric and asymmetric cryptography?

7. True or False: A hash can be de-hashed back to plain text.

8. What are the two keys in asymmetric cryptography called?

---

## Quick Reference

```
Cryptography Terms:
├── Plain text   → Readable text
├── Ciphertext   → Encrypted text
├── Encryption   → Plain text → Ciphertext
├── Decryption   → Ciphertext → Plain text
├── Cipher       → Algorithm for encryption/decryption
├── Block cipher → Encrypts data in blocks
├── Stream cipher → Encrypts continuous data
└── Key          → Data required by cipher

Hashing:
├── One-way process
├── Cannot be reversed
├── Collision-free
├── Also called: message digest, checksum, fingerprint
└── Uses: passwords, file verification, signatures

SHA-2 Tools:
├── sha224sum → 224-bit key
├── sha256sum → 256-bit key
├── sha384sum → 384-bit key
└── sha512sum → 512-bit key

Create Hash:
└── sha256sum filename

Verify Hash:
└── sha256sum -c hashfile.txt

Symmetric Ciphers:
├── AES (Rijndael) → 128-512 bit blocks/keys
├── Blowfish       → 64-bit blocks, 32-448 bit keys
├── CAST5          → 64-bit blocks, up to 128-bit keys
├── DES            → INSECURE (56-bit key)
├── 3DES           → Improved DES
├── IDEA           → 64-bit blocks, 128-bit key
├── RC4            → Stream cipher, variable key
├── RC5            → 32-128 bit blocks, up to 2048-bit keys
└── RC6            → Faster RC5

Asymmetric Ciphers:
├── RSA                    → Most popular
├── El Gamal               → Logarithm algorithm
├── Elliptic Curve (ECDSA) → Elliptic curve points
└── Ed25519                → More secure/faster than RSA

Symmetric vs Asymmetric:
├── Symmetric:
│   ├── Single key
│   ├── Same key for encrypt/decrypt
│   ├── Fast
│   └── Key sharing problem
└── Asymmetric:
    ├── Key pair (public/private)
    ├── Public key can be shared
    ├── Private key stays secret
    ├── More secure
    └── Slower

Key Size:
├── 56-bit  → Insecure (DES)
├── 128-bit → Secure
├── 256-bit → Very secure
└── 512-bit → Extremely secure

Encryption Uses:
├── Files
├── Partitions/volumes
├── Web connections (HTTPS)
├── Network connections (SSH/VPN)
├── Backups
├── Zip files
└── Email
```
