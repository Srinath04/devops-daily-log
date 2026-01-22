
### 🚢 Docker - Deepdive

Went beyond basic Docker usage and explored **how Docker actually works under the hood**.

- Studied **`dockerd`** as the core daemon that manages images, containers, networks, and volumes - understanding how every `docker run`, `docker ps`, or `docker start` is ultimately a **client → daemon interaction**.
    
- Traced the **container lifecycle** from request → daemon → container runtime, than treating containers as black boxes.
    
- Practically explored **container-to-container communication**, focusing on how containers discover and talk to each other **without host level hacks**.
    

**Hands-on examples :**

- Created multiple containers on the same Docker network and validated communication using container names instead of IPs.
    
- Observed how containers remain isolated by default, and how Docker networking selectively enables communication while still preserving namespace isolation.
    
- Verified communication paths and commands (`docker network inspect`, `ping`, `curl`, `run`, `exec`, `logs`) `/var/run/docker.sock`
    
This paved way for understanding container communication **intentional and controlled**.
