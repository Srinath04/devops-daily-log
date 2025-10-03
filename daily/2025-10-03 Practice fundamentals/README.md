
## Reinforced below topics indepth 

**Kubernetes API & Certificates**
    
- Compared **kubectl vs kubelet** roles in cluster communication.
        
- Explored in detail on **curl requirirement on certs** for API access, while kubectl can use base64-encoded secrets (how kubectl handles decoding automatically).
        
- Tested whether copying certs to a local laptop and running API requests would work → confirmed possible if certs and kubeconfig are properly configured.
        
- Broke down steps to understand **API communication flow in Kubernetes**, using Minikube as the sandbox.

**Pod Experiments**
    
- Checked methods to identify the **port an image listens on** (via Dockerfile `EXPOSE`, container docs, or inspecting inside pod with `ss`, `netstat`, or logs).
