
### Debugging VM Termination (Memory & Swap)

- Understood how **OOM (Out Of Memory)** conditions can cause VM or application termination even when CPU looks healthy.
    
- Learned the role of **swap memory** as a buffer when RAM is exhausted, and how **no / misconfigured swap** increases crash risk.
    
- Identified key checks: `free -h`,`free -mt`, `swapon`, `dmesg | grep -i oom`, and system logs to confirm memory related termination.
    
- Noted that excessive swapping can also degrade performance, making **swap sizing a balance**, not a fix.

---
### Terraform Variables

- Playing with **Terraform variables** decouple configuration from code for reusable and environment specific deployments.  

---

### Linux User Management (Non-Interactive & Expiry)

- create **non-interactive users** using shells like `/sbin/nologin` or `/bin/false`.
    
- Understood use cases: service accounts, automation users, and restricted access roles.
    
- Explored **account expiry** and password aging using `chage` for temporary or compliance driven access.
    
- Recognized expiry and non-login shells as **preventive security controls**, not just user restrictions.
