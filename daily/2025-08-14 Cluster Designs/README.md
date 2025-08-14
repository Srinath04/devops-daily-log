
## 🔧 Activity
Studied the theoretical basics of cluster design, high availability (HA) control planes, quorum in etcd clusters, and placement strategies (stacked vs external). Also explored how managed services and turnkey solutions handle provisioning.
## 🔍 Key Learnings
As generally known a Kubernetes cluster consists of control plane nodes and worker nodes.
        Control plane nodes manage cluster state and scheduling.
        Worker nodes run workloads (Pods).
High availability (HA) is achieved by having multiple control plane nodes instead of relying on a single master node.
        Multiple control plane nodes ensure no single point of failure.

etcd, the distributed key-value store, is at the heart of the control plane and requires quorum (majority of members) to make decisions.
etcd requires quorum (majority agreement) to function correctly. Example: In a 3-node etcd cluster, quorum is 2.

###etcd Deployment Options
- Stacked topology: etcd runs with the control plane nodes. Simpler to deploy but less resilient.
- External topology: etcd runs on dedicated nodes. More resilient but needs additional infra.

Stacked topology is simpler but ties etcd health to the control plane node health. External topology provides stronger isolation and reliability but requires extra infrastructure.
External topology: etcd runs on dedicated nodes. More resilient but needs additional infra.

###Provisioning Approaches

####Cloud Managed Service:
Key Players: Amazon EKS, Google GKE, Azure AKS.
Control plane and etcd are fully managed by the provider.

Users only manage worker nodes and workloads.
High availability and quorum are handled automatically.

####Turnkey Solutions:
- Tools like kOps, Rancher RKE, or Kubeadm automation scripts.
- Provide cluster bootstrapping with HA options.
- Require user management of both control planes and etcd, though tooling simplifies setup.

###💰 Cost-Saving Points
- Use stacked topology for non-critical or dev/test clusters to avoid extra nodes.
- For production, managed services (EKS/GKE/AKS) often reduce TCO by offloading ops overhead and avoiding downtime costs.
- Right-sizing worker nodes (autoscaling, spot instances in cloud) provides major savings while keeping performance.
- Turnkey solutions (like Rancher) can be cheaper for mid-sized companies than full managed services, since licensing/support may still cost less than 24/7 cloud SLA charges.
- Avoid overprovisioning etcd. 3 nodes are usually enough; 5 only when handling very large workloads.
