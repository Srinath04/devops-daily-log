
## Topics Covered:

1. Node Affinity in Persistent Volumes (PVs):

  - Learned how node affinity can be applied in PVs to ensure storage is only accessible from pods scheduled on specific nodes.
  - Useful for workloads that need data locality or node-specific hardware (e.g., SSDs on certain nodes).

2. Vertical Pod Autoscaler (VPA):

  - Understood how VPA works by adjusting CPU and memory requests/limits of pods automatically based on usage.
  - Compared with HPA: VPA adjusts resource sizes of existing pods, while HPA adjusts replica counts.

3. Secrets in Environment Variables:

  - Explored how Kubernetes secrets can be injected into pods via environment variables.
  - Example: defining a secret and then referencing it in a Deployment’s envFrom or env field.

  - Also noted potential risks since environment variables may appear in pod specs/logs.

4. Fixing Failed Kubelet Issue:

  - Investigated troubleshooting steps for kubelet failure.
  - Checked service logs via journalctl -u kubelet -f.
  - Verified version compatibility with apt show kubelet | grep Version before install/upgrade.
  - Common fixes: restarting kubelet, ensuring kubelet version matches kubeadm, checking node config, and resolving certificate or CNI plugin errors.

5. Debugged ingress bad gateway error during creation.

6. Verified PV and PVC matching rules (spec, namespace scope, access modes, storage class).

### Commands & References from Today’s Work:
```
journalctl -u kubelet -f → live logs for kubelet.

systemctl status kubelet → Unit kubelet.service could not be found.

apt show kubelet | grep Version → check installed kubelet version

crictl ps -a →check running and existed container from system pods

kubectl exec dnspod-ckapg-svcn -- curl testservice-ckapg-svcn

kubectl create secret generic db-secret-test -n playground --from-literal=DB_Host=mysql --from-literal=DB_User=root --from-literal=DB_Password=default123
```
