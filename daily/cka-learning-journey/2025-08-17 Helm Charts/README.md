
## 🔧 Activity
1. Explored Helm charts and their role in Kubernetes.
2. Practiced working with Helm to deploy applications.
3. Learned how to customize Helm chart parameters to suit application needs.
4. Understood lifecycle management with Helm, including installation, upgrades, and rollback of applications.

## 🔍 Key Learnings
- Helm Charts provide a reusable and version controlled way to package Kubernetes applications.
- Customization can be achieved by overriding default values through `` values.yaml ``  or `` --set flags ``

### Lifecycle management with Helm simplifies:
- Installing applications (helm install)
- Upgrading versions/configurations (helm upgrade)
- Rolling back to stable versions if needed (helm rollback).
- Helm enables consistency across environments by standardizing deployment templates.

## 🖥️  Commands / Configs
```
# Install a chart (example: NGINX)
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx

# View installed releases
helm list

# Customize parameters (using --set)
helm install my-nginx bitnami/nginx --set service.type=NodePort

# Upgrade release with new values
helm upgrade my-nginx bitnami/nginx -f custom-values.yaml

# Rollback to previous version
helm rollback my-nginx 1
```

#### 💡 Cost-Saving Angle
- Consistency reduces errors: fewer failed deployments mean less downtime and wasted compute.
- Reusable charts: avoid repetitive YAML writing, saving engineering time.
- Rollback capability: prevents prolonged outages, which indirectly saves operational costs.

### 🌍 Real Time Usecase
- A fintech team uses Helm charts to deploy their microservices across dev, staging, and production environments. Instead of writing separate YAML files for each environment, they override values through values-dev.yaml, values-staging.yaml, and values-prod.yaml. This ensures faster deployments, fewer errors, and consistent configurations.
