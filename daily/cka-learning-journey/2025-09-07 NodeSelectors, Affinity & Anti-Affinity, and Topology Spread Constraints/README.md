
Today’s focus was on **scheduling controls in Kubernetes**, specifically how Pods can be directed to run on particular nodes or be distributed evenly across the cluster.

- **Pods beyond containers**
    
    - While Pods are most commonly used to run containers, they represent a **general workload abstraction**.
    - With extensions like **KubeVirt**, a Pod can manage a **virtual machine (VM)** as a workload.
    - With projects like **Knative**, Pods can run **serverless functions**, scaling up and down as needed.
    - This shows Pods are not restricted to containers—they are the foundation for multiple workload types.
        
- **NodeSelectors**
    
    - Simplest way to constrain Pods to run on nodes with specific labels.
    - Example: `nodeSelector: {"disktype": "ssd"}` ensures the Pod only runs on nodes labeled with `disktype=ssd`.
        
- **Node Affinity**
    
    - More expressive and flexible than NodeSelectors.
    - Uses `requiredDuringSchedulingIgnoredDuringExecution` (hard rules) and `preferredDuringSchedulingIgnoredDuringExecution` (soft rules).
    - Example: Scheduling workloads on nodes in a certain availability zone.
        
- **Pod Affinity**
    
    - Ensures Pods are co-located with other Pods for better communication and performance.
    - Example: Running frontend Pods close to backend Pods in the same AZ.
        
- **Pod Anti-Affinity**
    
    - Ensures Pods are scheduled away from each other for high availability.
    - Example: Distributing replicas of the same application across different nodes to prevent single-node failure.
        
- **Topology Spread Constraints**
    
    - Enforce even distribution of Pods across failure domains (like zones, nodes, or regions).
    - Helps avoid hotspots by spreading Pods based on labels and topology keys.
    - Example: Ensuring 3 replicas of a Pod are spread across 3 different zones.
        
**Key takeaway:** Pods are the **universal workload unit** in Kubernetes—not just for containers, but also extensible to VMs and serverless. Scheduling rules ensure these workloads are highly available, fault-tolerant, and efficiently distributed.
