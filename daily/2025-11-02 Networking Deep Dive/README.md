
### **Ingress Deep Dive & Gateway API Introduction**

#### **1. Ingress – Deep Dive**

- **Ingress Purpose:**  
    Acts as an HTTP/HTTPS entry point for cluster services. It provides external access to internal services through a single load balancer or IP.
    
- **Core Components:**
    
    - **Ingress Resource:** Defines routing rules (host, path, backend service).
        
    - **Ingress Controller:** Implements these rules — examples: _NGINX Ingress Controller_, _Traefik_, _HAProxy_.
        
    - **IngressClass:** Binds specific Ingress resources to a controller (like `nginx`).
        
- **Key Concepts Explored:**
    
    - **`path` and `pathType`:**
        
        - `Prefix`: Matches based on path prefix (e.g., `/app` matches `/app/login`).
            
        - `Exact`: Matches only the exact path.
            
    - **`rewrite-target`:** Used to modify the path before forwarding to the backend service (e.g., `/pay` → `/`).
        
    - **Host-based routing:** Allows multiple domain mappings using a single IP/load balancer.
        
    - **Ingress + CoreDNS:** CoreDNS resolves internal service names; external DNS maps public domains to ingress IPs.
        
- **Flow**

    - Users access via domain → DNS → Ingress Controller (listens on NodePort/LB) → routes to appropriate Service → Pod.        

---

#### **2. Gateway API – Introduction**

- **Why Gateway API:**  
    A modern evolution of Ingress — designed for **more flexibility**, **role-based ownership**, and **better traffic management** (L4 + L7).
    
- **Key Resources:**
    
    - **GatewayClass:** Defines the implementation (e.g., managed by a specific controller).
        
    - **Gateway:** Represents a logical entry point (can map to load balancer or IP).
        
    - **HTTPRoute / TCPRoute / TLSRoute:** Define traffic rules (routing, filtering, security).
        
    - **ReferenceGrant:** Allows cross-namespace references securely.
        
- **Advantages over Ingress:**
    
    - Supports multiple listeners (ports/domains).
        
    - Can be managed by different teams (Infra vs. App teams).
        
    - Extensible model — supports future protocols beyond HTTP.
        
    - Works well with **service mesh** and **multi-tenant** environments.
        
- **Visual Flow:**
    
```
[GatewayClass] --> defines controller type
      ↓
[Gateway] --> load balancer entry
      ↓
[HTTPRoute] --> defines routing rules
      ↓
[Service / Pod] --> traffic destination
```
