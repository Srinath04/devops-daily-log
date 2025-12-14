
### Pod & Service Networking 

#### **1. System Mount Context**

- Explored **mount points** related to Kubernetes and container namespaces:
    
    `findmnt /sys`
    
    Output:
    
    `TARGET SOURCE FSTYPE OPTIONS /sys   sysfs  sysfs  rw,nosuid,nodev,noexec,relatime`
    
    This shows `/sys` is a **sysfs filesystem**, which exposes kernel and device info to user space.
    
    `sudo mount --make-shared /sys`
    → Ensures the `/sys` mount is **shared** between namespaces, allowing containers and the host to synchronize mount events 
    (important for container runtimes and kubelet operations).
    
---
#### **2. Root Filesystem Check**

- Verified filesystem structure using:
    
    `findmnt /`
    
    Useful for confirming **root mount propagation** and isolation between namespaces.

---

#### **3. Service & Pod Overlap Prevention**

- Investigated **service and pod IP overlap** — a common networking pitfall when cluster CIDRs are misconfigured.
    
- Used:
    
    `iptables -L -t nat | grep db-service`
    
    → Checked NAT table entries for the `db-service`, verifying **ClusterIP → Pod IP** translation and ensuring no conflicting DNAT rules exist.
    
- Learned how **iptables rules** under the `KUBE-SERVICES` and `KUBE-POSTROUTING` chains handle:
    
    - Service to Pod mapping
        
    - SNAT for external traffic
        
    - Prevention of IP overlaps between `--service-cidr` and `--pod-cidr`

---

#### **4. IPAM – Pod and Service Networking**

- Reviewed how **IP Address Management (IPAM)** assigns pod and service IPs:
    
    - **Pod network:** Managed by the **CNI plugin** (e.g., Flannel, Calico), using its configured **`--pod-cidr`** range.
        
    - **Service network:** Defined in **kube-apiserver** via `--service-cluster-ip-range`.
        
    - **Overlap prevention:** Ensuring these CIDRs never intersect to avoid routing confusion in `kube-proxy` and CoreDNS.
        
- Understood :
    
    - Pod IPs are **routable within the cluster**.
        
    - Service IPs are **virtual** and use **iptables (generally) ipvs rules** for traffic redirection.    
---

### **Key Takeaways**
    
- `iptables -t nat` is crucial to verify service forwarding rules.
    
- Always configure separate pod and service CIDRs to prevent overlap.
    
- IPAM is the silent coordinator between CNI plugins and Kubernetes API for stable networking.
