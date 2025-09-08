
Kubernetes & Container Runtime – Daily Documentation

#### Summary

- **changed system-level configs** (`sysctl`) to ensure networking stability.
- **validated runtime containers** without depending on Kubernetes APIs.
- **inspected logs at the container runtime** to confirm DNS health.

### 1. **IP Forwarding & Node Networking Validation**

- **Why:** Pod-to-Pod communication across nodes depends on Linux kernel IP forwarding. Without it, traffic routed by CNI plugins (flannel, calico, etc.) will silently drop.
    
- **Steps Taken:**

    - Checked current forwarding status:        
        `sysctl net.ipv4.ip_forward`
        
        → Initially set to `0` (disabled).
        
    - Enabled it temporarily:
    
        `sysctl -w net.ipv4.ip_forward=1`
        
    - Made it persistent by editing `/etc/sysctl.conf`:
        
        `net.ipv4.ip_forward=1`
        
    - Verified via:
        
        `ip route show | egrep '10\.|172\.|192\.' ip addr show flannel.1 2>/dev/null || ip addr | grep flannel`
        
- **Outcome:** Pod network routes became visible, and traffic flowed between namespaces as expected.
   
---
### 2. **Installing and Using `crictl` for Runtime Debugging**

- **Why:** In containerd-based clusters (common in CKA exams and production), if `docker ps` is unavailable or kubectl is not serving `crictl` is the direct CLI to the CRI API.
    
- **Setup:**
    
    - Installed `crictl` from release binaries.
        
    - Configured default runtime endpoint:
        
        `cat <<EOF | sudo tee /etc/crictl.yaml runtime-endpoint: unix:///run/containerd/containerd.sock image-endpoint: unix:///run/containerd/containerd.sock timeout: 10 debug: true EOF`
        
- **Commands Used:**
    
    - `crictl ps` → check running containers.
        
    - `crictl ps -a` → all containers, including terminated init containers (important for debugging CrashLoopBackOff).
        
    - `crictl logs <container-id>` → inspected CoreDNS logs to confirm service discovery queries.
        
    - `crictl inspect <container-id>` → detailed state and mount points.
        
- **Outcome:** Could confirm kube-proxy, CoreDNS, and kube-apiserver pods were running correctly at the runtime level, independent of `kubectl`.
    
---
### 3. **CoreDNS & cluster.local Alternatives**

- **Why:** Service discovery is a frequent issue in CKA and real clusters. Default domain is `cluster.local`, but custom domains can be set in kubelet/kubeadm configs.
    
- **Steps Taken:**
    
    - Verified resolution of Kubernetes API:
        
        `kubectl exec -it <pod> -- nslookup kubernetes.default.svc.cluster.local`
        
        → Successful resolution returned internal ClusterIP.
        
    - Explored alternative service names:
        
        - Short name: `kubernetes`
            
        - Namespace-qualified: `kubernetes.default`
            
        - FQDN: `kubernetes.default.svc.cluster.local`
            
    - Verified CoreDNS pods and logs:
        
        `crictl logs <coredns-container-id> | grep error`
        
    - Confirmed no misconfigurations in `Corefile`.
        
---
### Key Learnings & Insights

- **Networking:** IP forwarding must be explicitly verified on every node, especially when deploying on bare metal or non-managed clusters.
    
- **Runtime Debugging:** `crictl` is the first line of defense when `kubectl` is down. Knowing `ps -a` and `logs` commands makes troubleshooting unique from generic docs.
    
- **DNS Discovery:** Understanding how CoreDNS resolves short names → FQDN is critical. Deep knowledge of `cluster.local` and its alternatives helps in diagnosing broken service resolution.
    
---

