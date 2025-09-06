
## Today's Documentation 

Helm search, HPA stabilization, Node CPU/Memory Capacity
  
**Summary:** Short, practical notes covering: Helm search (how to find charts and inspect them), Horizontal Pod Autoscaler (HPA) stabilization behavior and tuning, and how to check and reason about CPU & memory capacity on cluster nodes. 

---

### 1) Helm — search, inspect, and evaluate charts

Use `helm search` to discover charts from configured repositories and inspect chart details before installation.

**Common commands**

- List repos:

```bash
helm repo list
```

- Update local cache:

```bash
helm repo update
```

- Search chart name across repos (stable + remote):
    
```bash
helm search repo <keyword>
#### example
helm search repo nginx
```

- Search charthub/remote (if plugin/registry configured):
    
```bash
helm search hub <keyword>
```

- Show chart details and values template:
    
```bash
helm show chart <repo/chart>
helm show values <repo/chart>   # default values.yaml
helm show readme <repo/chart>   # README.md from chart
```

- Download/inspect chart locally without installing:
    
```bash
helm pull <repo/chart> --untar
ls <chart-dir>
```

**Checklist before install**

- `helm show values` — examine resource requests/limits, probes, affinity, and important toggles.
- Confirm chart version and app version.
- Check container images and whether they point to trusted registries or allow override.
- Search for post-install hooks or CRD requirements in README.
- If customizing, prepare `values.yaml` and test with `helm template`:

```bash
helm template myrelease <repo/chart> -f values.yaml | kubectl apply --dry-run=client -f -
```

**Tips**

- Prefer pinned chart versions for reproducibility (`--version` flag).
- Use `helm lint` (locally) after modifying values or charts.

---
### 2) HPA (Horizontal Pod Autoscaler) — stabilization and behavior

 HPA scales the number of pod replicas based on observed metrics (CPU, memory, custom metrics). 
 Stabilization controls prevent rapid flip flopping of replica counts.

**Key params & their effects**

- `--horizontal-pod-autoscaler-sync-period` (controller setting) — how often the controller checks metrics (default 15s in many clusters; confirm your control-plane config).
    
- `spec.behavior` (v2 API) — allows `scaleUp` and `scaleDown` policies, each with `stabilizationWindowSeconds`, `policies` (type: Percent/Pods), and `selectPolicy`.
    
**Example HPA with stabilization**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web-pg-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: web-pg
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 75
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
      - type: Percent
        value: 50
        periodSeconds: 60
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 20
        periodSeconds: 60
```

**Stabilization rationale**

- `scaleDown.stabilizationWindowSeconds` prevents HPA from immediately reducing replicas after a short metric drop — avoids churn and ensures transient dips don't remove capacity.
 
- `scaleUp.policies` and `scaleDown.policies` control maximum change per period (e.g., cap percent or fixed pods).
    

**Troubleshooting HPA flapping**

- Increase `stabilizationWindowSeconds` for `scaleDown` (e.g., 300s) to reduce frequent scale-ins.
- Limit `scaleUp` to smaller percentages if upstream bursts cause too-large scale-ups.
- Check `kubectl describe hpa <name>` for events, last metrics, and recommendation history.
- Ensure metrics-server or custom metrics adapter is healthy; stale metrics cause wrong decisions.
    
**Commands**

```bash
kubectl get hpa
kubectl describe hpa web-hpa
kubectl top pods # to verify current CPU usage
kubectl top nodes
```

---

### 3) Node CPU & Memory resource capacity — inspection and reasoning

**What to check:** total allocatable CPU/memory on nodes, current usage, daemonsets & system reserved, and fragmentation (how many pods can fit given requests).

**Commands**

```bash
kubectl get nodes -o wide
kubectl describe node <node-name>    # shows Allocatable vs Capacity and system reservations
kubectl top nodes                    # shows real-time usage
kubectl get pods -A -o=custom-columns=NS:.metadata.namespace,NAME:.metadata.name,CPU:.spec.containers[*].resources.requests.cpu,MEM:.spec.containers[*].resources.requests.memory
```

**Key fields**

- `Capacity` vs `Allocatable` on node (Allocatable = Capacity minus kube-reserved / system-reserved / eviction thresholds).
    
- `kube-reserved` and `system-reserved` are configured on kubelet; check node description to learn what's reserved.
    

**How to compute how many pods can fit (simple model)**

1. Sum `allocatable` CPU (in millicores) and memory (Mi).
2. Sum requested resources for the workload pods.
3. `available = allocatable - sum(requests of system + running pods)`; `max_new_pods ≈ floor(available / request_per_pod)` (per resource, take the minimum of CPU-based and memory-based counts).
    

**Notes**

- Requests determine scheduling; limits do not. Ensure requests reflect realistic minimum needs to avoid overcommit and OOMs.
- Use PodDisruptionBudgets (PDBs) for critical apps so scaling/draining won't bring availability below desired level.
- Watch for `eviction` threshold events in kubelet logs: when memory is tight, kubelet may evict pods.
    
**Fragmentation & bin-packing**

- Small leftover CPU/memory slices across many nodes can prevent scheduling of larger pods even when total cluster capacity looks sufficient. Consider bin-packing strategies, node pools with different sizes, or vertical scaling of nodes.
    

**Commands for auditing capacity and schedulability**

```bash
# show allocatable vs used (requests) across cluster (simple script)
kubectl get nodes -o json | jq '.items[] | {name: .metadata.name, allocatable: .status.allocatable}'
# or use metrics-server + kube-state-metrics + Grafana for visual dashboards
```
