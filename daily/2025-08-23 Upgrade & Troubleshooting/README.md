
## 🔧 Activity
1. explored the process of upgrading Kubernetes clusters using kubeadm. I covered both the control plane and worker node upgrade steps, ensuring minimal disruption and version consistency across the cluster. I also reinforced some of my Kubernetes querying skills with real commands.
2. learned that jq is more powerful than JSONPath for JSON processing, especially for complex transformations and filtering.

## 🔍 Key Learnings
- Kubeadm upgrade is the standard way to upgrade Kubernetes clusters created with kubeadm.
- Upgrades need to be applied first to the control plane (API server, controller manager, scheduler, etc.) and then to the worker nodes.
- The upgrade ensures that cluster components, kubelet, and kubectl stay compatible.
- The process should be done sequentially, respecting Kubernetes version skew policies.
- Kubernetes queries are useful for debugging and validating resource states before and after upgrades.
- JQ vs JSONPath: JSONPath is good for quick field extraction, while jq provides scripting power for transformations, filtering, and reshaping JSON.

## 🖥️  Commands / Configs
### Controlplane 
```
# Check current cluster version
kubectl get nodes

# Upgrade kubeadm to the desired version
sudo apt-get update && sudo apt-get install -y kubeadm=<version>

# Plan the upgrade
sudo kubeadm upgrade plan

# Apply the upgrade to control plane components
sudo kubeadm upgrade apply v<version>

# Upgrade kubelet and kubectl
sudo apt-get install -y kubelet=<version> kubectl=<version>

# Restart kubelet
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```
### Worker node(s)
```
# Drain the node to safely evict workloads
kubectl drain <node-name> --ignore-daemonsets

# Upgrade kubeadm
sudo apt-get update && sudo apt-get install -y kubeadm=<version>

# Upgrade kubelet configuration
sudo kubeadm upgrade node

# Upgrade kubelet and kubectl
sudo apt-get install -y kubelet=<version> kubectl=<version>

# Restart kubelet
sudo systemctl daemon-reload
sudo systemctl restart kubelet

# Uncordon the node to schedule pods again
kubectl uncordon <node-name>
```
### Jsonpath queries
```
# Get nodes in JSON format
kubectl get no -o json

#Add custom column
k -n playground get deployments.apps -o custom-columns='DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace' --sort-by=.metadata.name

# Sort PersistentVolumes by storage capacity
kubectl get pv --sort-by=.spec.capacity.storage

# Extract only specific columns (first 2)
kubectl get pv -o custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage

# Filter context by user with JSONPath
kubectl config view -o jsonpath='{.contexts[?(@.context.user=="aws-user")].name}'
```

## Cost Saving Angle
- Following proper upgrade practices reduces risks of outages, avoiding indirect costs from downtime or instability.
- Practicing queries with jq and JSONPath helps optimize debugging efficiency, saving engineering time.
