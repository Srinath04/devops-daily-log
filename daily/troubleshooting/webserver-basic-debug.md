# Web Server Troubleshooting

## Scenario
A web server is running and a file exists at the default path, but `curl 127.0.0.1:80` returns nothing.

---

## Troubleshooting Approach (Bottom-Up)

### 1. Is the process running?
```bash
ps -ef
systemctl status apache2   # Ubuntu/Debian
systemctl status httpd     # RHEL/CentOS
```

### 2. Is it listening on the right port?
```bash
ss -tlnp | grep :80
```
This confirms the process is bound to port 80.

### 3. Is a firewall blocking traffic?
```
sudo iptables -L -n
```
Looked for DROP or REJECT rules on port 80.

> Yes found

**Fix — remove the blocking rule:**
```bash
sudo iptables -D INPUT -p tcp --dport 80 -j DROP
```

### 4. Check application logs
```bash
sudo tail -20 /var/log/apache2/error.log
sudo tail -20 /var/log/apache2/access.log
```

> Now the webpage is returned but says **Forbidden - You don't have permission to access this resource**

### 5. File permissions
As curl returns `403 Forbidden`, the web server user (`www-data`) can't read the file.
```
ls -la /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html
```

---

## Root Causes Found
| Issue | Symptom | Fix |
|---|---|---|
| iptables DROP rule on port 80 | curl hangs indefinitely | `iptables -D INPUT -p tcp --dport 80 -j DROP` |
| File permissions too restrictive | 403 Forbidden | `chmod 644 <file>` |

---

## Key Concepts

**Two types of firewalls to be aware of:**
- **OS-level (iptables/ufw):** runs on the server itself — can block traffic before it reaches the app
- **Network-level (AWS Security Groups, corporate firewall):** sits outside the server at the network edge

In production, both layers exist and both need to be checked.

**Layered troubleshooting mental model:**
```
Process running? → Listening on port? → Firewall allowing? → App responding? → File accessible?
```
