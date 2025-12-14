
## 📘 Today’s Learning Progress

## Topics Covered

1. Persistent Volume (PV) Mount in Pods

2. Explored how a pod can mount a Persistent Volume that is already in a Bound state.

3. Learned that even if the PV status is Bound, the application inside the pod may not display the expected data unless the correct mount path and access mode are configured.

4. Verified disk presence inside the pod but noted it wasn’t showing due to path mismatch or pod spec configuration.

5. Service for Multiple Pods

6. Understood the purpose of a Service in Kubernetes: abstracting access to multiple pod replicas.

7. Clarified that port (service-facing) and targetPort (container-facing) can differ, but the Service ensures traffic is properly routed to all matching pods.

8. Identified that Services decouple applications from the pod lifecycle.

9. kubectl explain Usage

10. Practiced kubectl explain for exploring API resources.

11. Understood its role in exam conditions as a quick in-cluster documentation tool when external docs restriction.
    
&emsp;Example: kubectl explain pod.spec.containers

12. Managing Critical Pods (like kube-scheduler)

13. Explored how to pause/stop/restart system pods temporarily for troubleshooting.

&emsp;These are usually managed by static pod manifests or DaemonSets, so editing/deleting is not be ideal choice.

## 📝 Test My Knowledge (Self-Check)

Q: If a PV is Bound but data is not visible inside the pod, what could be wrong?
A: Likely an incorrect mount path or access mode mismatch in the pod spec.

Q: Why does one Service object serve multiple pods?
A: Because Services use label selectors to group pods and provide stable access, decoupling clients from pod IP changes.

Q: How do I explore the fields available for Deployment.spec in an exam like situation without third party resource access?
A: Use kubectl explain deployment.spec and navigate further with .fieldName.

Q: If I want to temporarily stop the kube-scheduler, how would I do it?
A: Edit or move its static pod manifest (e.g., /etc/kubernetes/manifests/kube-scheduler.yaml) — it will stop automatically until restored.

## Key Takeaway

Today reinforced how core Kubernetes abstractions (PV, Service, kubectl tools, and system components) connect together. I also practiced answering self-check questions to simulate exam conditions and strengthen recall.
