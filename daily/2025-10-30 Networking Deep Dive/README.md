
## **Daily Progress – 30 Oct 2025 (Networking)**

### **Main Focus:**

**Docker Networking vs Kubernetes Networking (CNI-based Architecture)**

---
### 🐳 **Docker Networking Recap:**

- **Docker’s Default Network (Bridge Mode):**
    
    - When Docker is installed, it automatically creates a network interface named **`docker0`**, a Linux bridge.
        
    - Each container gets an IP from this bridge’s subnet (e.g., `172.17.0.0/16`), where `172.17.0.1` is typically assigned to `docker0`.
        
    - Docker handles IP allocation and NATing automatically.
        
    - Docker’s networking model is **host-local**, meaning containers on different hosts can’t directly communicate without explicit routing or overlays.
        

---
### **Kubernetes Networking (CNI-Based Model):**

- Kubernetes doesn’t create its own bridge like Docker; it delegates all pod to pod and node to node communication to a **CNI plugin**.
    
- When a pod is scheduled:
    
    1. The **Kubelet** calls the **CNI plugin** (binary in `/opt/cni/bin/`).
        
    2. The plugin config (in `/etc/cni/net.d/`) defines how to attach the pod to a network.
        
    3. The plugin sets up the pod’s network namespace, assigns an IP, and connects it to a virtual network (bridge, vxlan, or overlay).
        
    4. The plugin then reports the IP info back to Kubelet, which updates the Pod’s status.
        
---

### 🔗 **CNI Plugin Flow Example (Flannel or Calico):**

1. Pod created → Kubelet triggers CNI `ADD` call.
    
2. Flannel creates a **veth pair** — one end in the pod’s namespace, the other in the host’s.
    
3. Host-side veth attaches to a Flannel bridge or VXLAN device.
    
4. IP allocation handled via Flannel’s subnet manager or Calico’s IPAM.
    
5. Communication between nodes achieved through overlay (Flannel VXLAN) or routing (Calico BGP).

---
### 📁 **CNI Config Files:**

- Usually found under `/etc/cni/net.d/`
    
- Example (Flannel snippet):
    
    `{   "cniVersion": "0.3.1",   "name": "cbr0",   "type": "flannel",   "delegate": {     "bridge": "cni0",     "isDefaultGateway": true   } }`
    
- Some setups also chain multiple plugins — e.g., `flannel` for overlay + `portmap` for NAT.
    
- This “chaining” is why you may see multiple binaries (like `flannel` and `portmap`); the CNI runtime executes them sequentially.
    

---
### Insights: Network Namespaces in Context**

- Every Pod in Kubernetes runs inside its own **network namespace**.
    
- The namespace isolates network interfaces, routes, and IP tables.
    
- The CNI plugin links this namespace to the cluster network, making pod to pod communication seamless across nodes.
    
---

### **Takeaway:**

Docker provides a ready made bridge for container networking, while Kubernetes deliberately externalises this through CNI for **modularity and flexibility** — enabling diverse backends like Flannel, Calico, Cilium, etc., to handle complex cluster networking needs.
