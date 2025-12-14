
## Today's Learnings

### Helm Operations

1. Helm Lint: Used helm lint to validate chart structure and YAML syntax before installation. Helps identify misconfigurations early.
2. Helm Install / Uninstall: Practiced installing and uninstalling Helm charts. Learned to confirm deployments and cleanup post-uninstall.
3. Helm Debug: Used --debug flag during Helm operations to troubleshoot chart rendering and release issues.

### Kubernetes Ingress & HTTPRoute

- Explored Ingress objects for exposing services externally.
- Understood how HTTPRoute (as part of Gateway API) provides more granular routing, offering modern replacement/enhancement over Ingress.

### Compared Ingress vs HTTPRoute:

- Ingress: Simpler, widely supported, basic routing.
- HTTPRoute: More flexible, works with Gateways, advanced traffic policies.

### Rollout Management

- kubectl rollout history: Practiced checking rollout history of deployments to review revision states.
- Learned to identify stable vs failed rollouts and how to revert when necessary.

### Debugging Practice

- Applied various debugging approaches including verifying configs, ports, and routing rules.
- Learned structured troubleshooting using Helm debug output, kubectl describe, and logs.
