
### **Progress – October 10, 2025**

#### **Topics Covered**

1. **Encryption at Rest**
    
    - Understood the concept of securing Kubernetes data (secrets, config maps, etc.) stored on disk using encryption.
        
    - Reviewed how to enable **Encryption at Rest** by configuring `EncryptionConfiguration` in `kube-apiserver`.
        
    - Key focus points:
        
        - File path: `/etc/kubernetes/pki/encryption-config.yaml`
            
        - Common providers: `aescbc`, `aesgcm`, `secretbox`, and `identity`
            
        - API server flag:
            
            `--encryption-provider-config=/etc/kubernetes/pki/encryption-config.yaml`
            
        - Verified encryption status by checking stored secrets in `etcd` (base64 encrypted output).
            
2. **Multi-Container Pods**
    
    - Explored how a single pod can contain multiple containers sharing the **same network namespace** and **volumes**.
        
    - Understood practical use cases like:
        
        - Sidecar containers for logging or proxying
            
        - Helper containers for synchronization or shared data processing
            
    - Example YAML structure reviewed:
        
        `apiVersion: v1 kind: Pod metadata:   name: multi-container-pod spec:   containers:   - name: app     image: nginx   - name: sidecar     image: busybox     command: ['sh', '-c', 'while true; do echo Sidecar running; sleep 5; done']`
        
    - Practiced checking logs for individual containers using:
        
        `kubectl logs multi-container-pod -c sidecar`
        
3. **Init Containers**
    
    - Learned how **Init Containers** run sequentially before the main application containers.
        
    - Use cases covered:
        
        - Setting up environment prerequisites
            
        - Waiting for dependencies to be ready (e.g., database)
            
    - Key command practice:
        
        `apiVersion: v1 kind: Pod metadata:   name: init-pod spec:   initContainers:   - name: init-db     image: busybox     command: ['sh', '-c', 'echo waiting for db; sleep 10']   containers:   - name: main-app     image: nginx`
        
    - Observed init container lifecycle and transition using:
        
        `kubectl get pods -w kubectl describe pod init-pod`
        
---

### **Additional Notes**

- Reviewed relationships between **multi-container pods** and **init containers** — how init containers can prepare data or configuration for multi-container applications.
    
- Reinforced concepts of **data sharing using volumes** between containers.
