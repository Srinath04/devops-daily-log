
**Date:** 2025-09-16  

- Practice on **httpd header-based routing** and YAML structuring, 
	- `name` and `backendRef` were both practiced with array syntax
    
- **Storage concepts:** understanding the difference between `local` and `hostPath` volumes in Kubernetes
 
---
## Header-Based Routing

- Learned how nginx can be used for header-based routing.
    
- Requests with specific headers (like `X-Environment: canary`) can be routed to a different backend.
    
- Key takeaway: `rules`,` `matches`, and `headers` can achieve header-based routing.
    
- Practiced with lab examples of routing traffic to stable vs canary backends.
    
---

## YAML Structuring with Arrays

- Understood that in Kubernetes manifests, both `name` and `backendRef` fields can be defined using **array syntax** when specifying multiple backends.
    
- Practiced writing Ingress/service manifests where:
    ```
    rules:
  - matches:
    - headers:
        - name: X-Environment
          value: canary
    backendRefs:
    - name: web-service-canary
      port: 8080
  - backendRefs:
    - name: web-service
      port: 8080
    ```
  
- Emphasis: `name` can be part of a structured array, and `backendRef` can also use arrays to define multiple references.
    
---

## Storage: Local vs HostPath

- **hostPath**:
    
    - Mounts a file or directory from the **host node’s filesystem** into a Pod.    
    - Useful for testing but tightly coupled to the node where the pod runs.
    - If the pod is rescheduled to another node, the data may not be available.
        
    - Example:
    - ```
      volumes:
        - name: data
          hostPath:
            path: /data/app
            type: Directory
      ```
        
- **local**:
    
    - Introduced as a more production-oriented persistent storage option.
    - Tied to a specific node but integrates with **PersistentVolume** objects.
    - Works with the Kubernetes scheduler to ensure pods using a `local` volume are scheduled on the correct node.
    - Requires a `PersistentVolume` manifest, unlike hostPath.

Example:
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
local:
  path: /system/volumes/data
nodeAffinity:
  required:
    nodeSelectorTerms:
      - matchExpressions:
	- key: kubernetes.io/hostname
	operator: In
	values:
	  - control-plane
```
