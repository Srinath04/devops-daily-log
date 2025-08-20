
## 🔧 Activity
1. Diagnosed Kubernetes Service networking (ClusterIP, NodePort, and port mappings).
2. Troubleshot database connectivity in a Kubernetes-based web application.
3. Connected past learnings on Ingress and namespace design to today’s networking concepts.

## 🔍 Key Learnings
### Service Port Mapping

- targetPort must equal the container’s application port (e.g., MySQL listening on 3306).
- port is the cluster-internal port that clients (Pods) use.
- ClusterIP exposes the Service inside the cluster only.
- NodePort maps that Service externally via a port (30000–32767) on every node.

### Application Connectivity
- DB_HOST in a Deployment must equal the Service name (DNS within cluster).
- Mismatches (mysql vs mysql-service) cause real-world downtime — this kind of detail is critical in production troubleshooting.
- Services are immutable in name → renaming requires delete/recreate and redeployment updates.
- Networking misalignments in Kubernetes usually surface as app connection failures.
- Knowing how DNS, Services, and environment variables tie together shows skill beyond YAML writing — it shows capability to debug distributed systems.
