
**Progress Update – Networking Basics in Kubernetes (Oct 27, 2025)**

Today’s focus was on **networking fundamentals** with a Kubernetes perspective, covering both core concepts and their role inside a cluster:

- **Switches** – within Kubernetes, the **CNI (Container Network Interface)** plugin performs a role similar to a switch, enabling pods to communicate inside the same node or across nodes.
    
- **Routers** – the **kube-proxy** and **routing tables** handle pod-to-pod and pod-to-service traffic, deciding the network path for packets.
    
- **Gateways** – represent access points between internal and external networks, much like **Ingress** or **Gateway API** in Kubernetes that control inbound/outbound cluster traffic.
    
- **DNS & CoreDNS** – CoreDNS is the internal DNS server of Kubernetes. It automatically resolves **service names** (e.g., `my-service.default.svc.cluster.local`) to cluster IPs, ensuring service discovery within the cluster.
    
 **Takeaway:**  
Understood how traditional networking components map to Kubernetes networking — switches to CNIs, routers to kube-proxy routes, gateways to ingress controllers, and DNS to CoreDNS for internal name resolution.

