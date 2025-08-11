
## 🔧 Activity 
1. Practiced cross-namespace service access in Kubernetes using fully qualified DNS names.
2. Investigated Pod to Service DNS resolution inside Kubernetes clusters.
3. Reviewed Git push workflows (git push vs git push -u) for efficient remote tracking setup.
4. Tweaked terminal keybindings to improve shell navigation efficiency.

## 🔍 Key Learnings
Kubernetes DNS Format
``<service-name>.<namespace>.svc.cluster.local``

This resolution works cluster-wide unless blocked by a NetworkPolicy.

- logging of pod's details in DNS is optional but not default

DNS Debugging in Pods

``Use kubectl exec -it <pod> -- nslookup <service>.<namespace>`` to verify DNS records before troubleshooting network policies or service selectors.

Ingress in Namespaces

- Ingress resources are namespace-scoped; they route traffic only to services within the same namespace, whereas Ingress controller itself can be cluster-wide.

Git Upstream Tracking

``git push -u origin <branch>`` both pushes code and sets the branch’s upstream, making future pushes/pulls simpler.
