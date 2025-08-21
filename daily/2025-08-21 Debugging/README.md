
## 🔧 Activity

1. Practiced troubleshooting control plane and worker node issues in Kubernetes.
2. Debugged kubelet, kube-proxy, and kube-scheduler errors.
3. Investigated CNI networking issues (pods stuck in ContainerCreating, missing plugins).
4. Validated service to pod connections and fixed empty service endpoints.

## 🔍 Key Learnings
### Kubelet Debugging

Common cert path:
```
/var/lib/kubelet/config.yaml
/var/lib/kubelet/pki/
```

### Restart kubelet and check logs:
```
systemctl restart kubelet
journalctl -u kubelet -f
```

### Kube-proxy Configuration
Default config path: `` /var/lib/kube-proxy/config.conf ``

- Ensure correct API group in config (not plain v1, but kubeproxy.config.k8s.io/v1alpha1 or newer).

### Node Not Ready
Check node status:
```
kubectl get nodes
kubectl describe node <node-name>
```
- Connectivity to control plane can fail if wrong port is configured (instead of 6443).

### Scheduler CrashLoopBackOff
Inspect logs:
`` kubectl logs -n kube-system kube-scheduler-controlplane``
- Often caused by missing certs or invalid kubeconfig.

### CNI/Networking Issues
- Pods stuck in ContainerCreating → usually CNI missin or misconfigured.
`` kubectl get pods -n kube-system``
- If weave/flannel/calico pods not running →First to ensure IIPs allocated to workloads.

### Services & Endpoints
Service selectors must match pod labels:
```
kubectl describe svc <service>
kubectl get pods --show-labels
```
- Empty endpoints may have misaligned labels.

### Ports
- targetPort must match the container’s listening port (otherwise traffic dies).
- port can be different (gives you flexibility).
- NodePort exposes an additional fixed port (30000–32767) on each node for external access.

## Real World Use Case
### A fintech app running on Kubernetes

- If kubelet or kube-proxy fails → pods won’t schedule, user payments may get stuck.
- If service selectors are misconfigured → APIs appear “down” even though pods are healthy.
- Debugging fast prevents service downtime → saves revenue and protects customer trust.

### Cost-Saving Angle

- Skilled debugging reduces MTTR (Mean Time To Recovery) → directly cuts operational costs.
- Avoids over-provisioning: instead of scaling up blindly, fixing networking/config ensures optimal infra usage.
