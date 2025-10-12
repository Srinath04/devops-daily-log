
**Daily Documentation — October 12, 2025**

### **Topic: Cluster Upgrade**

- Learned how to **perform Kubernetes cluster upgrades** step by step.
    
- Understood the **control plane node upgrade** process using `kubeadm upgrade plan`, `kubeadm upgrade apply`, and updating `kubelet` and `kubectl`.
    
- Practiced **worker node upgrade** using `kubectl drain`, `apt update kubeadm/kubelet`, and `kubectl uncordon`.
    
- Noted the importance of **backing up etcd** before upgrading to avoid data loss.
    
- Understood **version skew policy** — control plane and kubelet can differ by **one minor version** only.
    
- Reviewed how to **verify upgrade completion** via `kubectl get nodes -o wide` and `kubelet --version`.
    
- Observed behavior of **Pods and system components** during upgrade and how **drain/cordon** ensure workload safety.
    

📘 _Today’s session focused on mastering cluster lifecycle management via upgrades — a critical part of the CKA exam._

