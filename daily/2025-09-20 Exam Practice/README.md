
## Daily CKA Practice Documentation – 20 Sept 2025

### some of sections covered

1. **Cluster Init with `kubeadm-config` and Pod CIDR**
    
    - Practiced initializing the control plane with a custom Pod network CIDR.
        
    - Key command:
      
        `kubeadm init --pod-network-cidr=10.244.0.0/16`
        
    - Verified the config with:
        
        `kubeadm config view`
        
    - Ensured CNI plugin (Flannel/Calico) matches the CIDR used.
        
2. **HTTP Routing with Weight (Ingress/HTTPRoute)**
    
    - Configured traffic splitting between two backends using weight distribution.
        
    - Example snippet (`HTTPRoute`):
        
        `backendRefs:   - name: service-v1     port: 80     weight: 70   - name: service-v2     port: 80     weight: 30`
        
    - Tested routing behavior with repeated `curl` to validate weighted round-robin.
        
3. **Horizontal Pod Autoscaler (HPA) based on Requests**
    
    - Created an HPA scaling on CPU utilization.
        
    - Example:
        
        `kubectl autoscale deployment myapp --cpu-percent=50 --min=2 --max=10`
        
    - Validated scaling by generating CPU load with:
        
        `kubectl run -it load-generator --image=busybox /bin/sh`
        
        and running:
        
        `while true; do wget -q -O- http://myapp; done`
        
    - Confirmed pod count increase with `kubectl get hpa`.
        
---
