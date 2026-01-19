# Apache SSL/TLS Certificates

## SSL/TLS Overview

**Purpose:** Encrypt web traffic between client and server

**Protocols:**
*   SSL (Secure Sockets Layer) - Older, deprecated
*   TLS (Transport Layer Security) - Current standard
*   Often called "SSL" for both

**Port:** 443 (HTTPS)

**Use cases:**
*   E-commerce
*   Online banking
*   Login pages
*   Any sensitive data

---

## mod_ssl Package

**Installation:**
```bash
yum install mod_ssl crypto-utils
```

**Configuration file:**
*   `/etc/httpd/conf.d/ssl.conf`

**Default behavior:**
*   Self-signed certificate created at install
*   Located: `/etc/pki/tls/certs/localhost.crt`
*   Private key: `/etc/pki/tls/private/localhost.key`

---

## SSL/TLS Process

**Handshake:**
1. Client connects to server (port 443)
2. Server sends certificate
3. Client verifies certificate
4. Session key negotiated
5. Encrypted communication begins

**Encryption types:**
*   **Asymmetric** (public key) - During handshake
*   **Symmetric** (session key) - For data transfer

---

## Certificate Components

**Private key:**
*   Kept secret on server
*   Used to decrypt data
*   Never shared

**Public key:**
*   Included in certificate
*   Shared with clients
*   Used to encrypt data

**Certificate:**
*   Contains public key
*   Server information
*   Validity period
*   Digital signature

---

## Certificate Authorities (CA)

**Purpose:** Verify server identity

**Commercial CAs:**
*   InstantSSL (https://www.instantssl.com)
*   Let's Encrypt (https://www.letsencrypt.org) - Free!
*   DigiCert (https://www.digicert.com)

**Options:**
1. **Commercial CA** - Trusted by browsers
2. **Self-signed** - For testing/internal use
3. **Own CA** - For small organizations

---

## Generating RSA Private Key

**Create key directory:**
```bash
cd /etc/pki/tls/private
```

**Generate 2048-bit key:**
```bash
openssl genrsa -out server.key 2048
```

**Set permissions:**
```bash
chmod 600 server.key
```

**With passphrase (more secure):**
```bash
openssl genrsa -des3 -out server.key 2048
```

**⚠️ Note:** Passphrase required at Apache startup

---

## Creating Self-Signed Certificate

**Generate certificate:**
```bash
cd /etc/pki/tls/certs
openssl req -new -x509 -nodes -sha256 -days 365 \
  -key /etc/pki/tls/private/server.key \
  -out server.crt
```

**Provide information:**
```
Country Name (2 letter code) [AU]: US
State or Province Name (full name) [Some-State]: NJ
Locality Name (eg, city) []: Princeton
Organization Name (eg, company) []: Example Company
Organizational Unit Name (eg, section) []: IT Department
Common Name (eg, YOUR name) []: www.example.com
Email Address []: admin@example.com
```

**⚠️ Important:** Common Name must match server hostname

**Options explained:**
*   `-new` - New certificate
*   `-x509` - Self-signed
*   `-nodes` - No passphrase
*   `-sha256` - SHA-256 hash
*   `-days 365` - Valid for 1 year

---

## Creating Certificate Signing Request (CSR)

**For commercial CA:**

**Create CSR:**
```bash
mkdir /etc/pki/tls/ssl.csr
cd /etc/pki/tls/ssl.csr
openssl req -new -key ../private/server.key -out server.csr
```

**Provide information:**
```
Country Name (2 letter code) [AU]: US
State or Province Name (full name) [Some-State]: Washington
Locality Name (eg, city) []: Bellingham
Organization Name (eg, company) []: Example Company, LTD.
Organizational Unit Name (eg, section) []: Network Operations
Common Name (eg, YOUR name) []: secure.example.com
Email Address []: admin@example.com

Please enter the following 'extra' attributes
A challenge password []:
An optional company name []:
```

**Submit CSR to CA:**
1. Visit CA website
2. Request certificate
3. Copy/paste CSR content
4. CA validates information
5. CA sends signed certificate

**Save certificate:**
```bash
vi /etc/pki/tls/certs/example.com.crt
```

(Paste certificate from CA)

---

## Configuring SSL in Apache

**Edit `/etc/httpd/conf.d/ssl.conf`:**

**Basic SSL VirtualHost:**
```apache
Listen 443 https

<VirtualHost _default_:443>
    ServerName www.example.com:443
    
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/server.crt
    SSLCertificateKeyFile /etc/pki/tls/private/server.key
    
    DocumentRoot /var/www/html
    
    ErrorLog logs/ssl_error_log
    TransferLog logs/ssl_access_log
    LogLevel warn
</VirtualHost>
```

**Multiple SSL sites (requires separate IPs):**
```apache
Listen 192.168.1.100:443 https
Listen 192.168.1.101:443 https

<VirtualHost 192.168.1.100:443>
    ServerName www.example.com
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/example.com.crt
    SSLCertificateKeyFile /etc/pki/tls/private/example.com.key
    DocumentRoot /var/www/html/example.com
</VirtualHost>

<VirtualHost 192.168.1.101:443>
    ServerName www.example.org
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/example.org.crt
    SSLCertificateKeyFile /etc/pki/tls/private/example.org.key
    DocumentRoot /var/www/html/example.org
</VirtualHost>
```

**⚠️ Important:** Each SSL site needs unique IP address

---

## Testing SSL Configuration

**Check syntax:**
```bash
apachectl configtest
```

**Restart Apache:**
```bash
systemctl restart httpd.service
```

**Test from browser:**
```
https://www.example.com
```

**Self-signed certificate warning:**
*   Browser shows security warning
*   Click "Advanced"
*   View certificate details
*   Accept risk and continue

---

## Viewing Certificate

**From command line:**
```bash
openssl x509 -in /etc/pki/tls/certs/server.crt -text -noout
```

**Check expiration:**
```bash
openssl x509 -in /etc/pki/tls/certs/server.crt -noout -dates
```

---

## Review Questions

1. What port does HTTPS use?

2. What package provides SSL support for Apache?

3. What command generates an RSA private key?

4. What is a CSR?

5. True or False: Self-signed certificates are trusted by browsers.

6. What directive enables SSL in a VirtualHost?

7. Why do SSL sites need separate IP addresses?

8. Where should private keys be stored?

---

## Quick Reference

```
Installation:
└── yum install mod_ssl crypto-utils

Ports:
├── HTTP:  80
└── HTTPS: 443

Generate Private Key:
└── openssl genrsa -out server.key 2048

Self-Signed Certificate:
openssl req -new -x509 -nodes -sha256 -days 365 \
  -key /etc/pki/tls/private/server.key \
  -out /etc/pki/tls/certs/server.crt

Certificate Signing Request:
openssl req -new \
  -key /etc/pki/tls/private/server.key \
  -out /etc/pki/tls/ssl.csr/server.csr

SSL VirtualHost:
<VirtualHost *:443>
├── ServerName www.example.com
├── SSLEngine on
├── SSLCertificateFile /etc/pki/tls/certs/server.crt
├── SSLCertificateKeyFile /etc/pki/tls/private/server.key
└── DocumentRoot /var/www/html
</VirtualHost>

File Locations:
├── Certificates: /etc/pki/tls/certs/
├── Private keys: /etc/pki/tls/private/
├── CSRs: /etc/pki/tls/ssl.csr/
└── Config: /etc/httpd/conf.d/ssl.conf

Testing:
├── apachectl configtest
├── systemctl restart httpd.service
└── https://localhost

View Certificate:
├── openssl x509 -in cert.crt -text -noout
└── openssl x509 -in cert.crt -noout -dates
```
