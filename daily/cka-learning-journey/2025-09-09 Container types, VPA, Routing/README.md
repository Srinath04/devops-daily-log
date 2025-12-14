
### Co-Located Containers, Sidecar Pattern, VPA Adjustments & Header-Based Routing via Matches

#### 1. **Co-Located Containers**

- Pods in Kubernetes allow multiple containers to run side by side, sharing:
    
    - **Network namespace** → containers talk over `localhost`.
    - **Volumes** → shared filesystem access.
    - **Lifecycle** → all containers restart together.
        
- Typical usage: app + helper container (log shipper, monitoring agent, proxy).
  
#### 2. **Sidecar Pattern**

- A **sidecar** extends the functionality of the main container without modifying it.

- Key examples:
    
    - **Service mesh proxies** (Envoy/Istio).        
    - **Log forwarders** (Fluent Bit, Vector).
    - **Data sync/backup agents**.
        
- Benefit: decouples responsibilities → app team doesn’t need to change core business logic

#### 3. **Vertical Pod Autoscaler (VPA) – CPU & Memory**

- Adjusts **CPU/memory requests & limits** based on observed runtime usage.
    
- Components:
    - **Recommender** – watches usage, suggests optimal resources.
    - **Updater** – evicts & recreates Pods with adjusted requests.
    - **Admission Controller** – applies recommendations on new Pod creation.
        
- Use case: prevent **OOMKilled** errors and save node capacity by avoiding oversized requests.

#### 4. **Header-Based Routing with Matches**

- Instead of annotations, modern routing APIs allow **explicit header-based matching**.
    
- Example (Gateway API `HTTPRoute`):
  
    `apiVersion: gateway.networking.k8s.io/v1beta1 kind: HTTPRoute metadata:   name: header-routing spec:   parentRefs:   - name: my-gateway   rules:   - matches:     - headers:       - name: X-Environment         value: playground     backendRefs:     - name: playground-svc       port: 80   - matches:     - headers:       - name: X-Environment         value: production     backendRefs:     - name: prod-svc       port: 80`
    
- Behavior:
  
    - Requests with `X-Environment: playground` → sent to **playground-svc**.        
    - Requests with `X-Environment: production` → sent to **prod-svc**.
      
- Benefits:
    
    - Safer playground rollouts and controlled experiments.
    - Avoids relying on less transparent ingress controller snippets.
    - Declarative and portable across controllers supporting Gateway API.
---
