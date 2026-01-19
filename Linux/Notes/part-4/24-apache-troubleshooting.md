# Apache Troubleshooting

## Configuration Testing

**Check syntax:**
```bash
apachectl configtest
```

**Expected output:**
```
Syntax OK
```

**Graceful restart (tests config first):**
```bash
apachectl graceful
```

**Benefits:**
*   Tests configuration before applying
*   Doesn't disconnect active clients
*   Reloads configuration smoothly

---

## Common Configuration Errors

### Port Already in Use

**Error message:**
```
[crit] (98)Address already in use: make_sock: could not bind to port 80
```

**Causes:**
*   Another Apache instance running
*   Another service using port 80
*   Duplicate Listen directives

**Check what's using port:**
```bash
netstat -tupln | grep :80
```

**Example output:**
```
tcp6  0  0  :::80  :::*  LISTEN  2105/httpd
```

**Solutions:**
*   Stop other service
*   Kill duplicate Apache process
*   Remove duplicate Listen directives

### Syntax Errors

**Error example:**
```
Syntax error on line 42 of /etc/httpd/conf/httpd.conf:
Invalid command 'DocumentRot', perhaps misspelled
```

**Fix:** Correct typo in configuration file

### Missing Module

**Error example:**
```
Cannot load modules/mod_ssl.so into server
```

**Solution:**
```bash
yum install mod_ssl
```

---

## Log Files

### Error Log

**Location:** `/var/log/httpd/error_log`

**View recent errors:**
```bash
tail -f /var/log/httpd/error_log
```

**Search for specific error:**
```bash
grep "error" /var/log/httpd/error_log
```

**Common error levels:**
*   `[emerg]` - Emergency (server unusable)
*   `[alert]` - Alert (immediate action required)
*   `[crit]` - Critical
*   `[error]` - Error
*   `[warn]` - Warning

### Access Log

**Location:** `/var/log/httpd/access_log`

**Format:**
```
192.168.1.100 - - [18/Sep/2019:10:30:45 -0400] "GET /index.html HTTP/1.1" 200 1234
```

**Fields:**
*   IP address
*   User (if authenticated)
*   Timestamp
*   Request
*   Status code
*   Bytes sent

**View recent access:**
```bash
tail -f /var/log/httpd/access_log
```

---

## Increasing Log Level

**Edit httpd.conf:**
```apache
LogLevel debug
```

**Levels (least to most verbose):**
*   `emerg`
*   `alert`
*   `crit`
*   `error`
*   `warn` (default)
*   `notice`
*   `info`
*   `debug`

**⚠️ Warning:** debug creates large log files

**After troubleshooting, return to warn:**
```apache
LogLevel warn
```

---

## Permission Errors

### File Permissions

**Error:**
```
[error] [client 192.168.1.100] (13)Permission denied: access to /index.html denied
```

**Check permissions:**
```bash
ls -l /var/www/html/index.html
```

**Fix:**
```bash
chmod 644 /var/www/html/index.html
chown apache:apache /var/www/html/index.html
```

**Directory permissions:**
```bash
chmod 755 /var/www/html
```

### SELinux Denials

**Error in log:**
```
[error] [client 192.168.1.100] (13)Permission denied
```

**Check SELinux:**
```bash
getenforce
```

**Temporarily disable:**
```bash
setenforce 0
```

**If it works, SELinux is the problem**

**Re-enable:**
```bash
setenforce 1
```

**Fix file context:**
```bash
restorecon -R -v /var/www/html
```

**Check for denials:**
```bash
grep AVC /var/log/audit/audit.log | tail
```

**Or:**
```bash
ausearch -m avc -ts recent
```

---

## Access Denied Errors

**Error:**
```
[error] [client 192.168.1.100] client denied by server configuration
```

**Causes:**
*   Require directive denying access
*   Directory/Location block restrictions

**Check configuration:**
```apache
<Directory /var/www/html>
    Require all denied  # Problem!
</Directory>
```

**Fix:**
```apache
<Directory /var/www/html>
    Require all granted
</Directory>
```

---

## Directory Index Forbidden

**Error:**
```
[error] [client 192.168.1.100] Directory index forbidden by rule
```

**Causes:**
*   No index file found
*   Indexes option disabled

**Solutions:**

**Add index file:**
```bash
echo "Welcome" > /var/www/html/index.html
```

**Or enable directory listings:**
```apache
<Directory /var/www/html>
    Options +Indexes
</Directory>
```

---

## 404 Not Found

**Error in browser:**
```
Not Found
The requested URL /page.html was not found on this server.
```

**Check:**
*   File exists
*   Correct path
*   Correct DocumentRoot
*   Correct VirtualHost

**Debug:**
```bash
ls -l /var/www/html/page.html
```

**Check error log:**
```bash
tail /var/log/httpd/error_log
```

---

## 500 Internal Server Error

**Causes:**
*   Script errors
*   .htaccess errors
*   Permission problems
*   SELinux denials

**Check error log:**
```bash
tail /var/log/httpd/error_log
```

**Common issues:**
*   PHP syntax errors
*   Perl/Python script crashes
*   Invalid .htaccess directives

---

## Debugging with httpd -X

**Run in foreground (debug mode):**
```bash
httpd -X
```

**Shows:**
*   Startup messages
*   Error messages
*   Debug output

**⚠️ Note:** Only one process, no forking

**Stop:** Ctrl+C

---

## Testing Tools

**Check if Apache is running:**
```bash
systemctl status httpd.service
ps -ef | grep httpd
```

**Check listening ports:**
```bash
netstat -tupln | grep httpd
```

**Test from command line:**
```bash
curl http://localhost
curl -I http://localhost  # Headers only
```

**Test SSL:**
```bash
curl -k https://localhost  # -k ignores cert errors
```

**Check DNS:**
```bash
nslookup www.example.com
dig www.example.com
```

---

## Review Questions

1. What command tests Apache configuration?

2. Where is the Apache error log located?

3. What does LogLevel debug do?

4. How do you check what's using port 80?

5. What command shows SELinux denials?

6. True or False: apachectl graceful tests config before reloading.

7. What error code indicates "Not Found"?

8. How do you run Apache in debug mode?

---

## Quick Reference

```
Configuration Testing:
├── apachectl configtest  → Test syntax
└── apachectl graceful    → Reload (tests first)

Log Files:
├── /var/log/httpd/error_log  → Error log
└── /var/log/httpd/access_log → Access log

View Logs:
├── tail -f /var/log/httpd/error_log
└── grep "error" /var/log/httpd/error_log

Log Levels:
├── emerg, alert, crit, error
├── warn (default)
└── notice, info, debug

Common Errors:
├── Port in use    → netstat -tupln | grep :80
├── Permission     → chmod 644 file, check SELinux
├── Access denied  → Check Require directives
├── 404 Not Found  → Check file exists, DocumentRoot
└── 500 Internal   → Check error_log, script errors

SELinux Troubleshooting:
├── getenforce              → Check mode
├── setenforce 0            → Disable temporarily
├── restorecon -R -v /path  → Fix contexts
└── ausearch -m avc -ts recent → Check denials

Testing:
├── curl http://localhost
├── curl -I http://localhost
├── netstat -tupln | grep httpd
└── systemctl status httpd.service

Debug Mode:
└── httpd -X  (Ctrl+C to stop)

Fix Permissions:
├── chmod 644 files
├── chmod 755 directories
└── chown apache:apache files
```
