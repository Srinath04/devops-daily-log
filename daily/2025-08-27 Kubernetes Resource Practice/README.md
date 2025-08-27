
---

## 🚀 Activities Covered

* **Service Discovery & DNS Testing:** Created a temporary BusyBox pod to test DNS resolution against cluster services and confirmed how CoreDNS maps service names to cluster IPs.
* **Pod Replication Behavior:** Investigated scenarios where multiple pods exist even when not explicitly expected, linked to Deployment replica settings and crash-restart policies.
* **Resource Quotas & Capacity Planning:** Deep-dived into namespace-level quotas (`Hard` and `Used`) and connected them to hardware realities such as dual-core CPUs vs virtualized cloud instances.
* **Cluster Hardware Mapping:** Explored the relation between on-prem bare-metal CPU families and equivalent cloud offerings in terms of vCPUs and memory allocations.

---

## 🛠 Commands / Configs Practiced

```bash
# Test DNS resolution using a throwaway BusyBox pod
kubectl run test-dns --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service

# Verify service resolution output (expected cluster DNS)
kubectl exec -it <pod> -- nslookup <service-name>

# Resource Quota inspection
kubectl describe resourcequota my-quota

# Understand allocations (Hard vs Used)
NAME         CREATED AT
my-quota     2025-08-27T12:10:00Z

```

---

## 💡 Key Learnings

* **DNS in Kubernetes:** DNS is fully managed by CoreDNS, and pods resolve services using `<service>.<namespace>.svc.cluster.local`. Testing with BusyBox ensures DNS propagation is verified at pod-level.
* **Replica Behavior:** Multiple pods are often expected when using Deployments, where the `replicas` field defaults to `1` but can scale automatically via HPA or manually. A restart policy can also cause multiple pod instances if older ones are terminating.
* **Quota Management:** The `Hard` value in a quota is the strict upper bound, while `Used` grows dynamically as pods request resources. This ensures predictable scheduling and avoids node overcommitment.
* **Hardware Abstraction:** Kubernetes normalizes compute units as `millicores` (mCPU). A dual-core laptop with 2000m CPU is roughly comparable to a small cloud VM, while higher-end CPUs (Ryzen/i9) map to larger instance types. This understanding aids in cost-to-performance comparison.

---

### 🌍 Real-World Use Case

* **DNS Validation in Microservices:** Ensuring service resolution avoids costly production outages where microservices fail to communicate internally. Engineers often deploy BusyBox or Netshoot pods as a diagnostic step.
* **Log Redirection in CI/CD:** Capturing pod deletion or rollout logs using `>>` ensures historic logs are preserved, vital for compliance audits and RCA (root cause analysis).
* **Quotas for Multi-Tenancy:** In a SaaS environment, quotas prevent one tenant from exhausting resources, maintaining fair usage while enabling predictable billing.
* **Cloud Cost Strategy:** Comparing bare-metal cores to vCPUs allows DevOps engineers to right-size cloud clusters, reducing overspending while ensuring workload performance.


