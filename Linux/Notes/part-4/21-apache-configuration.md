# Apache Configuration

## httpd.conf Structure

**Location:** `/etc/httpd/conf/httpd.conf`

**Format:** Directive-based configuration

**Sections:**
*   Global settings
*   Main server configuration
*   Virtual hosts

---

## Configuration Directives

**Directive types:**

**Simple directives:**
```apache
ServerRoot "/etc/httpd"
Listen 80
User apache
Group apache
```

**Container directives:**
```apache
<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

---

## Key Global Directives

**ServerRoot:**
```apache
ServerRoot "/etc/httpd"
```
*   Base directory for configuration files

**Listen:**
```apache
Listen 80
Listen 192.168.1.100:80
Listen *:80
```
*   Port and IP to listen on

**Include:**
```apache
Include conf.d/*.conf
```
*   Include other configuration files

**User and Group:**
```apache
User apache
Group apache
```
*   User/group for Apache processes

---

## DocumentRoot

**Purpose:** Root directory for web content

**Default:**
```apache
DocumentRoot "/var/www/html"
```

**Example:**
*   URL: `http://example.com/page.html`
*   File: `/var/www/html/page.html`

**Changing DocumentRoot:**
```apache
DocumentRoot "/home/user/public_html"
```

---

## Directory Containers

**Purpose:** Apply settings to specific directories

**Basic example:**
```apache
<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

**Root directory (deny all):**
```apache
<Directory />
    AllowOverride none
    Require all denied
</Directory>
```

**Options directive:**
*   `Indexes` - Generate directory listings
*   `FollowSymLinks` - Follow symbolic links
*   `ExecCGI` - Allow CGI execution
*   `Includes` - Allow server-side includes

**AllowOverride:**
*   `None` - Ignore .htaccess files
*   `All` - Allow .htaccess overrides
*   Specific: `AuthConfig`, `FileInfo`, `Indexes`

**Require directive:**
*   `Require all granted` - Allow everyone
*   `Require all denied` - Deny everyone
*   `Require host example.com` - Allow specific host
*   `Require ip 192.168.1.0/24` - Allow IP range

---

## DirectoryIndex

**Purpose:** Default files to serve for directories

**Example:**
```apache
DirectoryIndex index.html index.php index.htm
```

**Behavior:**
*   URL: `http://example.com/docs/`
*   Tries: `index.html`, then `index.php`, then `index.htm`
*   If none found: Shows directory listing (if Indexes enabled)

---

## ScriptAlias

**Purpose:** Map URL to CGI script directory

**Example:**
```apache
ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
```

**Usage:**
*   URL: `http://example.com/cgi-bin/script.cgi`
*   File: `/var/www/cgi-bin/script.cgi`

---

## Virtual Hosts

**Purpose:** Host multiple websites on one server

**Name-based virtual hosting:**
```apache
<VirtualHost *:80>
    ServerName www.example.com
    ServerAlias example.com
    ServerAdmin webmaster@example.com
    DocumentRoot /var/www/html/example.com
    DirectoryIndex index.php index.html
    
    ErrorLog logs/example.com-error_log
    CustomLog logs/example.com-access_log combined
</VirtualHost>
```

**IP-based virtual hosting:**
```apache
<VirtualHost 192.168.1.100:80>
    ServerName www.example.com
    DocumentRoot /var/www/html/example.com
</VirtualHost>
```

**Key directives:**
*   `ServerName` - Primary hostname
*   `ServerAlias` - Alternate hostnames
*   `ServerAdmin` - Admin email
*   `DocumentRoot` - Content directory
*   `ErrorLog` - Error log location
*   `CustomLog` - Access log location

**⚠️ Important:** First VirtualHost is default for unmatched requests

---

## User Web Directories

**Purpose:** Allow users to publish content from home directories

**Enable mod_userdir:**
```apache
<IfModule mod_userdir.c>
    UserDir enabled chris
    UserDir public_html
</IfModule>
```

**Directory permissions:**
```apache
<Directory "/home/*/public_html">
    Options Indexes Includes FollowSymLinks
    Require all granted
</Directory>
```

**Usage:**
*   User: `chris`
*   Directory: `/home/chris/public_html`
*   URL: `http://example.com/~chris`

**Requirements:**
*   Execute permission on `/home` and `/home/chris`
*   Proper SELinux context (if enforcing)

---

## ErrorDocument

**Purpose:** Custom error pages

**Examples:**
```apache
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
ErrorDocument 500 /error/500.html
```

**Error codes:**
*   `403` - Forbidden
*   `404` - Not Found
*   `500` - Internal Server Error

---

## Logging

**Access log:**
```apache
CustomLog "logs/access_log" combined
```

**Error log:**
```apache
ErrorLog "logs/error_log"
```

**Log formats:**
*   `combined` - Detailed (includes referrer, user agent)
*   `common` - Standard format
*   Custom formats can be defined

**LogLevel:**
```apache
LogLevel warn
```

Options: `emerg`, `alert`, `crit`, `error`, `warn`, `notice`, `info`, `debug`

---

## .htaccess Files

**Purpose:** Per-directory configuration

**Enable:**
```apache
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

**Filename:**
```apache
AccessFileName .htaccess
```

**Example .htaccess:**
```apache
Options +Indexes
DirectoryIndex index.php
ErrorDocument 404 /404.html

<RequireAll>
    Require ip 192.168.1.0/24
</RequireAll>
```

**⚠️ Performance:** .htaccess files checked on every request

---

## Review Questions

1. What is the default DocumentRoot in RHEL/Fedora?

2. What directive includes other configuration files?

3. What does `Require all granted` do?

4. How do you enable directory listings?

5. What is the purpose of ServerName in a VirtualHost?

6. True or False: The first VirtualHost is the default.

7. What file allows per-directory configuration?

8. What directive sets the default index files?

---

## Quick Reference

```
Main Config:
└── /etc/httpd/conf/httpd.conf

Key Directives:
├── ServerRoot "/etc/httpd"
├── Listen 80
├── User apache
├── Group apache
├── DocumentRoot "/var/www/html"
└── Include conf.d/*.conf

Directory Container:
<Directory /path>
├── Options Indexes FollowSymLinks
├── AllowOverride None
└── Require all granted
</Directory>

Virtual Host:
<VirtualHost *:80>
├── ServerName www.example.com
├── ServerAlias example.com
├── ServerAdmin admin@example.com
├── DocumentRoot /var/www/html/example.com
├── ErrorLog logs/example-error_log
└── CustomLog logs/example-access_log combined
</VirtualHost>

User Directories:
├── UserDir enabled username
├── UserDir public_html
└── URL: http://host/~username

Options:
├── Indexes        → Directory listings
├── FollowSymLinks → Follow symlinks
├── ExecCGI        → Execute CGI
└── Includes       → Server-side includes

Require:
├── Require all granted
├── Require all denied
├── Require host example.com
└── Require ip 192.168.1.0/24

Logging:
├── ErrorLog logs/error_log
├── CustomLog logs/access_log combined
└── LogLevel warn
```
