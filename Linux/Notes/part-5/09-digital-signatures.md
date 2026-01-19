# Digital Signatures and File Verification

## What is a Digital Signature

**Definition:**
*   Electronic originator for authentication
*   Verifies file source
*   Detects file modification
*   NOT a scanned physical signature

**Purpose:**
*   Prove file came from you
*   Ensure file not modified
*   Non-repudiation
*   Data integrity

---

## How Digital Signatures Work

**Creation process:**

**1. Create file/message**

**2. Create hash (message digest):**
*   GPG creates hash of file
*   One-way cryptographic process

**3. Encrypt hash with private key:**
*   Uses asymmetric cipher
*   Private key encrypts hash
*   Creates digital signature

**4. Send encrypted hash + file:**
*   Both sent to receiver
*   Encrypted hash = digital signature

**Verification process:**

**5. Receiver re-creates hash:**
*   Creates new hash of received file

**6. Decrypt digital signature:**
*   Uses sender's public key
*   Obtains original hash

**7. Compare hashes:**
*   Original hash vs new hash
*   If match = signature valid
*   File authentic and unmodified

**8. Read decrypted file**

---

## Digital Signature Ciphers

**Specialized ciphers:**
*   Some ciphers do both encryption and signatures
*   Some only create signatures

**Popular signature ciphers:**

**RSA:**
*   Can encrypt and sign
*   Most popular historically

**DSA (Digital Signature Algorithm):**
*   Signatures only
*   Cannot encrypt

**Ed25519:**
*   More secure than RSA
*   Faster than RSA
*   Modern choice

**ECDSA:**
*   Better protection than DSA
*   Elliptic curve based

---

## Signing Files with GPG

### Creating a Digital Signature

**Sign a file:**
```bash
gpg2 --output JohnDoe.DS --sign MessageForJill.txt
```

**Options:**
*   `--output filename` - Signed file output
*   `--sign` - Create digital signature

**Process:**
1. Creates message digest (hash)
2. Encrypts hash (creates signature)
3. Encrypts message file
4. Combines into output file

**Result:**
*   Encrypted and digitally signed file
*   Contains both signature and encrypted message

---

### Verifying a Digital Signature

**Check signature and decrypt:**
```bash
gpg2 --decrypt JohnDoe.DS
```

**Output:**
```
I am the real John Doe!
gpg: Signature made Sun 27 Oct 2019 07:03:21 PM EDT
gpg:                using RSA key 7469BCD3D05A43130F1786E0383D645D9798C173
gpg: Good signature from "John Doe <jdoe@example.com>" [unknown]
```

**Process:**
1. Decrypts message
2. Verifies digital signature
3. Displays message
4. Reports signature status

**Signature statuses:**
*   **Good signature** - Valid, unmodified
*   **Bad signature** - Invalid or modified
*   **No signature** - File not signed

---

### Sign and Encrypt for Privacy

**Problem:**
*   `--sign` alone allows anyone with public key to decrypt
*   Not truly private

**Solution:**
*   Use both `--sign` and `--encrypt`
*   Encrypt with recipient's public key
*   Only recipient can decrypt

**Command:**
```bash
gpg2 --output encrypted_signed --recipient "Recipient Name" --sign --encrypt message.txt
```

**Result:**
*   Signed with your private key
*   Encrypted with recipient's public key
*   Only recipient can decrypt
*   Recipient can verify your signature

**Recipient decrypts:**
```bash
gpg2 --decrypt encrypted_signed
```

---

## Clear-Sign Messages

**Purpose:**
*   Sign without encrypting
*   Message remains readable
*   Signature appended

**Create clear-signed message:**
```bash
gpg2 --clearsign message.txt
```

**Result:**
*   Creates `message.txt.asc`
*   Plain text message
*   Signature block at end

**Example output:**
```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

This is my message.
-----BEGIN PGP SIGNATURE-----

iQEzBAEBCAAdFiEEdGm808BaQxMPF4bgOD1kXZeYwXMFAl20FfkACgkQOD1kXZeY
...
-----END PGP SIGNATURE-----
```

**Verify:**
```bash
gpg2 --verify message.txt.asc
```

---

## Detached Signatures

**Purpose:**
*   Signature in separate file
*   Original file unchanged
*   Useful for software distribution

**Create detached signature:**
```bash
gpg2 --detach-sign filename
```

**Result:**
*   Creates `filename.sig`
*   Original file unchanged
*   Signature in separate file

**Verify detached signature:**
```bash
gpg2 --verify filename.sig filename
```

---

## Use Cases for Digital Signatures

**Software distribution:**
*   Verify software authenticity
*   Ensure no tampering
*   Common for downloads

**Email:**
*   Prove sender identity
*   Detect message modification
*   Non-repudiation

**Documents:**
*   Legal documents
*   Contracts
*   Official communications

**Code signing:**
*   Verify code author
*   Ensure code integrity
*   Trust verification

---

## Review Questions

1. What is a digital signature?

2. What are the steps to create a digital signature?

3. What cipher was historically most popular for signatures?

4. What command signs a file with GPG?

5. How do you verify a digital signature?

6. What's the difference between --sign and --clearsign?

7. True or False: A digital signature is a scanned image of your signature.

8. What is a detached signature?

---

## Quick Reference

```
Digital Signature:
├── Electronic authentication
├── Verifies file source
├── Detects modification
└── Uses asymmetric cryptography

Creation Process:
1. Create file
2. Create hash of file
3. Encrypt hash with private key (= signature)
4. Send file + signature

Verification Process:
1. Receive file + signature
2. Create new hash of file
3. Decrypt signature with public key (= original hash)
4. Compare hashes
5. Match = valid signature

Signature Ciphers:
├── RSA      → Encrypt and sign
├── DSA      → Sign only
├── Ed25519  → Modern, secure, fast
└── ECDSA    → Better than DSA

GPG Signing:
├── Sign: gpg2 --output file.sig --sign file.txt
├── Verify: gpg2 --decrypt file.sig
├── Clear-sign: gpg2 --clearsign file.txt
└── Detached: gpg2 --detach-sign file.txt

Sign and Encrypt:
└── gpg2 --output encrypted --recipient "Name" --sign --encrypt file.txt

Clear-Sign:
├── Creates: file.txt.asc
├── Message readable
├── Signature appended
└── Verify: gpg2 --verify file.txt.asc

Detached Signature:
├── Creates: file.sig
├── Original file unchanged
├── Signature separate
└── Verify: gpg2 --verify file.sig file

Signature Status:
├── Good signature  → Valid, unmodified
├── Bad signature   → Invalid or modified
└── No signature    → Not signed

Use Cases:
├── Software distribution
├── Email authentication
├── Legal documents
└── Code signing

Best Practices:
├── Always verify signatures
├── Check key fingerprints
├── Use trusted key servers
├── Keep private key secure
└── Revoke compromised keys
```
