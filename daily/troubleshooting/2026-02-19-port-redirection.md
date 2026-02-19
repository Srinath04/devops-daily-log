#  iptables Port Redirection 
 **Category:** Linux Networking / iptables

### Problem:  
A simulation app runs on an ephemeral port which cannot be changed.
A cron job doing a health check on localhost 80 expects it.

### Goal 
Make port 80 traffic reach the app - without touching the app config.

## Fix

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 41480
sudo iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-port 41480

# Verify
curl localhost:80/simulate
```

---
## What I Learned

### 1. Privileged Ports
Basics:
Ports below 1024 require root or CAP_NET_BIND_SERVICE.
That's why the simulation app runs on any of ephemeral port — safe zone for non-root.

### 2. iptables Tables
| Table  | Purpose                    |
| ------ | -------------------------- |
| filter | Allow / block traffic      |
| nat    | Redirect / translate ports |
| mangle | Modify packet headers      |

checked `iptables --list` showed only **filter table only**.  
used `-t nat` for NAT rules.

### 3. PREROUTING vs OUTPUT - The Key Insight

| Chain      | When it fires                      | Example                             |
| ---------- | ---------------------------------- | ----------------------------------- |
| PREROUTING | Traffic arriving from outside      | Another server hitting my port 80   |
| OUTPUT     | Traffic my local machine generates | `curl localhost:80` on same machine |

**PREROUTING alone won't work for localhost traffic — OUTPUT is required.**

## Other Debug Commands 
```
ss -tlnp                         # What's listening on which port
sudo iptables -t nat -L -n -v    # Check NAT rules
sudo simulator status            # Verify app is simulator-managed
```
