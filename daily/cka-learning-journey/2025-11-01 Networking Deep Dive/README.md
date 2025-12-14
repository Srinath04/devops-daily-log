
### **Daily Progress — 1 Nov 2025**

**Topics Covered:**

- **DNS in Kubernetes**
    
    - Understood how **Kubernetes uses CoreDNS** for internal name resolution.
        
    - Learned that every Pod can resolve Services using **cluster DNS (e.g., `my-service.default.svc.cluster.local`)**.
        
    - Reviewed how **CoreDNS runs as a Deployment** in the `kube-system` namespace and uses a **ConfigMap** to manage DNS rules.
        
    - Explored how CoreDNS interacts with **Kubelet’s /etc/resolv.conf** and the **cluster DNS IP** defined in the kubelet config.
        
- **CoreDNS Configuration**
    
    - Discussed key CoreDNS plugins like `kubernetes`, and `forward`
        
    - Observed how to troubleshoot DNS using `kubectl exec` and commands like:
        
        `kubectl exec -it web -- nslookup mysql.playground`
        
    - Learned the difference between **ClusterIP DNS resolution** and **ExternalName services**.
        
- **Introduction to Ingress**
    
    - Understood the **purpose of Ingress** for routing HTTP/HTTPS traffic to services.
        
    - Learned about **Ingress Controller** (e.g., NGINX, Traefik) as a component that enforces Ingress rules.
        
    - Got a high-level idea of **Ingress resource structure**:
        
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-service
            port:
              number: 80
```
