
## 🔧 Activity
1. Learned Kustomize fundamentals and how it differs from Helm.
2. Practiced **Base and Overlay directory structure** for managing multiple environments.
3. Explored **patching strategies**: Strategic Merge Patch and JSON 6902 Patch.
4. Used `` kustomize build `` and `` kubectl apply -k `` for manifest management.

## 🔍 Key Learnings
### Problem Statement
- Managing Kubernetes manifests at scale becomes complex. Copying YAML files per environment (dev, staging, prod) leads to:
- Duplication and drift across environments.
- Human error when maintaining updates.
- Difficulty in auditing changes.

### Kustomize Ideology
- Kustomize solves this by:
- Keeping a Base directory with common definitions (Deployments, Services, ConfigMaps).
- Allowing Overlays per environment to apply small deltas (e.g., replicas = 2 in dev, 5 in prod).
- Using patches to make controlled, minimal changes without duplicating entire manifests.

## Kustomize vs Helm
- Helm: Chart-based, good for packaging and distribution, but adds a templating engine (extra abstraction, requires learning values.yaml).
- Kustomize: Native kubectl integration, patch-and-overlay driven, simpler for teams that want plain YAML without templating.

### Patching Strategies

- Strategic Merge Patch: Extend/replace fields at YAML level (e.g., change replica count, image).
- JSON 6902 Patch: Fine-grained edits, useful for fields that require JSON path precision.

### Cost Saving Angle

- Reduced YAML duplication lowers storage and repo maintenance costs (especially in enterprise GitOps setups with hundreds of services).
- Leaner CI/CD pipelines: kustomize build produces pure manifests, so no need for Helm client/server setup or Tiller-like components.
- Easier onboarding: new engineers need only basic YAML skills instead of Helm’s templating language. This reduces training costs and speeds up delivery.

### Real world Usecase

Enterprise setup:
  Base directory defines core service (app + DB + secrets).  
  Overlays:  
   Dev: 1 replica, debug logging, local DB endpoint.  
   Staging: 2 replicas, staging DB, realistic config.  
   Prod: 5 replicas, autoscaling enabled, prod DB.  
  Updates: Change image version in Base, and it automatically flows to all overlays. Only overlay-specific differences remain unique.  

This ensures consistency across environments and eliminates “works in staging but not in prod” problems.

## 🖥️  Commands / Config
```
# Build and apply dev overlay
kubectl apply -k overlays/dev

# Just render manifests (helpful in CI/CD)
kustomize build overlays/prod > final.yaml

# Example directory structure
.
├── base
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays
    ├── dev
    │   ├── kustomization.yaml
    │   └── patch-replicas.yaml
    ├── staging
    │   ├── kustomization.yaml
    │   └── patch-replicas.yaml
    └── prod
        ├── kustomization.yaml
        └── patch-resources.yaml
```

