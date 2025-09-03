## Today's Kubernetes Learnings

1. Pod Run with Node Selection

- Worked with scheduling pods onto specific nodes using nodeSelector.
- This ensures workloads are placed on nodes that meet business or application requirements.

- Experimented with labels and taints to fine-tune pod placement strategies, reinforcing the importance of aligning scheduling rules with cluster architecture.

2. Network Policies (Targeting Selected Pods)

- Explored Kubernetes Network Policies to secure communication between pods.

- Designed ingress and egress rules for selected pods based on labels, ensuring only the intended services could communicate.

This exercise highlighted how fine-grained network controls reduce the attack surface and enforce a zero-trust model within the cluster.

3. ConfigMap from File for Centralized Configuration

- Created a ConfigMap directly from external configuration files to decouple configuration from code.

- Mounted ConfigMaps into pods both as environment variables and as mounted files, validating multiple consumption patterns.

This approach allows for dynamic reconfiguration without rebuilding container images, showcasing best practices for managing configuration in production systems.

4. ClusterIP Service from Pod Labels for Service Discovery

- Implemented a ClusterIP Service that automatically discovers backend pods using labels.

- Verified service connectivity by accessing the service endpoint from within the cluster.

Reinforced how Kubernetes Services abstract dynamic pod IPs and provide stable endpoints, which is critical for microservice architectures.
