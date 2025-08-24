
## 🔧 Activity
1. Attempted to work on etcd cluster backup but could not fully validate the process.
2. Recap on some of reacent learning in cka certification pov 

## 🔍 Key Learnings
### Etcd as the Source of Truth
- Etcd stores all Kubernetes cluster state (pods, secrets, config maps, API objects).
- A proper backup/restore strategy is essential for disaster recovery.

### Backup Reminders
- Backups require correct access to etcd pods or the control plane node.
- Validation of the snapshot is as important as taking it.

### Cluster Upgrade Process (Recap)
- kubeadm upgrade apply (control plane) and kubeadm upgrade node (workers).
- Understanding upgrade flow ensures safe migration across versions.

### Kustomize & Transformers (Recap)
- Image transformers, patches, and overlays enable environment-specific configuration without maintaining multiple YAMLs.

### JQ vs JSONPath (Recap)
- jq is more powerful for pipeline processing and debugging cloud-native systems.
Example: easily slicing arrays, extracting fields, filtering contexts.

## 🖥️  Commands / Configs
###Etcd Backup Attempt

``` # Take snapshot (example path, assuming etcdctl installed)
ETCDCTL_API=3 etcdctl snapshot save /tmp/etcd-snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
```
# Verify snapshot
``` ETCDCTL_API=3 etcdctl snapshot status /tmp/etcd-snapshot.db ```

### JSON Querying Example
```
# List all nodes in JSON, pipe through jq for names
kubectl get nodes -o json | jq '.items[].metadata.name'

# Sort PVs by capacity
kubectl get pv --sort-by=.spec.capacity.storage
```
### Upgrade Flow Reminder
```
# Control plane upgrade
sudo kubeadm upgrade apply v1.33.x

# Worker node upgrade
sudo kubeadm upgrade node
```
