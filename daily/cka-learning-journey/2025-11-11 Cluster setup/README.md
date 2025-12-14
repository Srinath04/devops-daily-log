
**Topic:** GCP VM Kubernetes Setup

- Reconfiguring a **multi-node Kubernetes cluster on GCP VMs**.
- As i have noticed an issue where **Calico pod** for worker node 2  alone remained in a non-ready restart state while others functioned correctly.
    
    - Verified that the issue was **not related to CIDR or control-plane configuration**, since other nodes operated normally.
        
    - After diagnosing cause suspected to be **node-specific Calico component failure** (BIRD config)
	    - to be verified if residual CNI configurations and incomplete interface initialization is the actual cause
        
    - **cleanly reinstalling Calico**, ensuring old CNI directories and network interfaces were cleared before redeployment.
    
- Used `gcloud compute instances describe` to validate **instance metadata**, including VPC, subnet, and internal IP mappings, ensuring correct network association for the cluster.
    
- Fine-tuned the **`.zshrc` configuration** for `gcloud` CLI — confirming proper PATH setup and shell autocompletion using
