
### Work Done Today:

Focused on practicing CKA practice areas. The session was structured as hands-on re-validation on core components

---

###  1. crictl Usage (Container Runtime Debugging)

- Verified container runtime via `crictl ps -a` and `crictl images`.
    
- Inspected container details:
    
    `crictl inspect <container_id> | less crictl logs <container_id>`
    
- Compared output against `kubectl describe pod` and `kubectl logs`.
    
-  Outcome: Strengthened runtime-level debugging skills beyond kubectl.
    

---

###  2. Pod Networking (Flannel / CNI)

- Checked network interfaces and routes:
    
    `ip addr | egrep 'flannel|cni0|vxlan' ip route | egrep '10\.|172\.'`
    
- Practiced pod-to-pod communication with `kubectl exec … ping`.
    
-  Outcome: Refreshed workflow for isolating pod-to-pod connectivity issues.
    
---

###  3. Ingress Header-based Routing

- Deployed two nginx versions (v1, v2).
    
- Configured ingress with:
    
    `nginx.ingress.kubernetes.io/configuration-snippet: |   if ($http_x_environment = "canary") {     set $service_name v2;   }`
    
- Validated using curl with/without custom headers:
    
    `curl -H "X-Environment: canary" http://<ingress-ip> curl http://<ingress-ip>`
    
- Outcome: Practiced advanced ingress rule usage with header-based routing.
    
---

### 4. Labels & Annotations

- Labeled pod with `env=dev` and verified:
    
    `kubectl label pod mypod env=dev kubectl get pods --show-labels`
    
- Annotated pod with metadata:
    
    `kubectl annotate pod mypod owner="srinath" kubectl describe pod mypod | grep -A2 Annotations`
    
-  Outcome: Clear difference reinforced between **labels** (selectors) and **annotations** (metadata only).
