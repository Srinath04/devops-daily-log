## ✅ Topics Covered

1. Debugging kubeconfig and port issues

2. Troubleshooting ReplicationController configurations

3. Usage of specific kubeconfig files

4. Identifying cluster-wide Pod CIDR range for CNI 

5. Basics of HTTP routing in Kubernetes

6. PersistentVolume (PV) and PersistentVolumeClaim (PVC) relationship
Matching criteria: storage class, access modes, capacity, etc.

## 🔑 Key Learnings

- Kubeconfig debugging: Ports and file paths are common breaking points; validating with explicit flags helps isolate issues.
- ReplicationControllers: Misconfigured selectors or labels often break replication; systematic debugging improves reliability.
- Cluster-wide Pod CIDR: Useful for understanding networking design, troubleshooting CNI plugins, and debugging pod to pod communication.
- HTTP routing: Reinforced understanding of ingress/gateway routing for service exposure.
- PV vs PVC: Matching happens only if specs align; realizing the scope difference (cluster-wide vs namespace) prevents misconfiguration in multi-team setups.

## 🌍 Real-World Relevance

- Debugging kubeconfig and ports mirrors multi-cluster or multi-context operations in real enterprise environments.
- Knowing PV/PVC scope difference is essential when multiple tenants or namespaces share cluster storage.
- CIDR and routing knowledge aids in cloud cost optimization, ensuring efficient IP utilization and avoiding network overlaps.

