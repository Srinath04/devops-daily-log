
## Topics Covered

1. Rolling Updates
2. Multi‑Container Pods
3. Init Container Troubleshooting
4. Kubeconfig Management
5. PersistentVolumeClaims (PVC)

### Rolling Updates

#### Purpose: Update applications without downtime.

```
kubectl set image deployment/myapp myapp=nginx:1.27 --record
kubectl rollout status deployment/myapp
kubectl rollout history deployment/myapp
kubectl rollout undo deployment/myapp
```

- Key Learning: Always verify rollout status.

### Multi‑Container Pods

Use Case: Sidecars for logging, monitoring, or proxy.

Manifest Example:
```
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-example
spec:
  containers:
    - name: main
      image: nginx
    - name: sidecar
      image: busybox
      command: ["sh", "-c", "while true; do echo Sidecar; sleep 5; done"]
```
- Key Learning: Multi‑container Pods share network and volumes. Useful when containers must tightly cooperate.

### Init Container Troubleshooting

- Observation: Init containers run sequentially before app containers.

#### Debugging Steps:
```
kubectl describe pod <pod-name>
kubectl logs <pod-name> -c <init-container>
```
Key Learning: Crash in init container blocks Pod startup.
Issue: container start command is incorrect


### Kubeconfig Management

Purpose: Store cluster connection info (clusters, users, contexts).

Commands:
```
kubectl config get-contexts
kubectl config use-context <context-name>
kubectl config view --minify --flatten
```
- Key Learning: Organizing kubeconfig avoids mistakes when switching between clusters.

### PersistentVolumeClaim (PVC)

- Flow: PV → PVC → Pod mount.

Manifest Example:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-test
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
- Key Learning: A PVC binds only when matching PV exists. Must check kubectl get pvc for Bound status.

## Key Learnings

- Rolling updates give zero‑downtime upgrades but must monitor rollout carefully.
- Multi‑container Pods are powerful for sidecar patterns but should not replace microservices separation.
- Init containers are excellent for setup tasks but require good error visibility.
- Kubeconfig hygiene is critical in multi‑cluster/multi‑user environments.
- PVCs decouple app storage requests from backend implementation — a good abstraction for portability.

Cost Saving: Multi‑container Pods minimize separate VM/Pod costs for tightly coupled tasks.
