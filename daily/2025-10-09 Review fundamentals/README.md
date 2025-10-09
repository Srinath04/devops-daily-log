
### Daily Progress – October 9, 2025**

**Topics Covered:**

- **Deployment Updates**
    
    - Practiced **`update`** **`rollingupdate`** and **`recreate`** strategies in Kubernetes.
        
    - Observed how changes to image versions and replica counts propagate gradually using `RollingUpdate` strategy.
        
    - Used commands like:
        
       ```kubectl rollout status deployment local-test   
        kubectl rollout history deployment local-test   
        kubectl rollout undo deployment/local-test  
        kubectl set image deployment/local-test lemon=nginx:1.28.0 --record```
        
    - Validated zero-downtime behavior during updates and checked pod status transitions.
        
- **Commands and Arguments**
    
    - Explored container `command` and `args` fields and their override hierarchy:
        
        - **Docker:** `ENTRYPOINT` and `CMD`
            
        - **Kubernetes:** `command` overrides Docker’s `ENTRYPOINT`, and `args` overrides Docker’s `CMD`.
            
    - Example:
        `command: ["sleep"] args: ["3600"]`
        
    - Tested combinations in `busybox` and custom images to verify precedence behavior.
        
- **Secrets**
    
    - Created and managed **Kubernetes Secrets** for environment variables and mounted volumes.
        
    - Practiced:
        
        `kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=pass123 kubectl get secrets db-secret -o yaml`
        
    - Applied secrets as:
        
        - **Environment Variables**
            
        - **Mounted Files** under `/etc/secrets`
            
    - Verified base64 encoding and decoding in secret manifests.
