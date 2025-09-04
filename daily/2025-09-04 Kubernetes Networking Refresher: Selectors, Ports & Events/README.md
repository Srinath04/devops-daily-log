Today’s Learning Update

Focus Area: Kubernetes – Networking, Policies, and Service Configuration

## Key Learnings

- Network Policies with NamespaceSelector & PodSelector:
Explored how NamespaceSelector and PodSelector can be used individually or in combination.

- NamespaceSelector lets you apply rules across entire namespaces.
- PodSelector filters traffic at a pod level using labels.

### When combined:

 - AND logic ensures that both namespace and pod criteria are satisfied.

 - OR logic (via multiple rules) allows flexibility by permitting traffic that matches either selector.
This clarified how Kubernetes enables precise traffic isolation between workloads.

## Use of spec.ports.name:
Learned why naming ports in service definitions (spec.ports.name) is crucial.
 - It allows containers with multiple ports to reference a specific port by name (e.g., when defining probes or network policies).

 - In advanced setups like Istio or NetworkPolicies, port names bring clarity and prevent misrouting.

### Service Testing with Pod Execution:
Used kubectl exec -- sh -c 'curl …' to verify if a pod could connect to services. Understood how -- sh -c allows complex shell commands, useful for connectivity debugging inside pods.

## ConfigMaps from Files:
Practiced creating and consuming ConfigMaps sourced from external files. This strengthens the separation of code and configuration, making applications more portable and easier to maintain.

### Field Selectors for Events (--field-selector):
Gained insight into using kubectl get events --field-selector involvedObject.name=<object-name> to filter events related to a specific resource (e.g., pod, node, or other object types). This is particularly useful for targeted troubleshooting instead of parsing cluster-wide events.

### ClusterIP Services with Pod Labels:
Reinforced understanding of how Kubernetes services route traffic by selecting pods via labels. Consistent labeling ensures smooth traffic routing and stable service discovery.

### Pod Lifecycle & Restart Policies:
Reviewed how --restart=Never impacts pod lifecycle, particularly for short-lived or job-style workloads, where retries are not desired.

## Takeaway

Today’s work reinforced my knowledge of fine-grained Kubernetes networking controls. Understanding the nuances of selectors, port naming, and event filtering helps design secure, resilient, and debuggable workloads in real-world production clusters.
