## 🔧 Activity

1. Explored Kustomize transformers for customizing Kubernetes manifests.
2. Learned about image transformers in Kustomize for updating container images and tags.
3. Studied patches in Kustomize and their types.
4. Worked with overlays to manage environment-specific configurations.
5. Understood how components extend Kustomize functionality.

## 🔍 Key Learnings
- Transformers in Kustomize are plugins that modify Kubernetes resources declared in manifests without touching the base files.

### Image Transformer:
- Helps replace image names, tags, or digests across manifests.
- Useful in CI/CD pipelines where image versions frequently change.

### Patches in Kustomize:

- Allow selective modifications of resources while keeping the base manifest intact.

### Types of patches:
- Strategic Merge Patch: Merge fields into an existing resource (most common).
- JSON Patch: Use explicit operations (add, remove, replace).
- Custom Overlays: Layered modifications applied via patchesStrategicMerge or patchesJson6902.

### Overlays:
- Provide a way to create environment-specific (dev, staging, prod) variants from a single base configuration.
- Reduce duplication while ensuring consistency.

### Components in Kustomize:
- Reusable building blocks that can be added to overlays.
- Useful for enabling optional features (e.g., monitoring, logging) without modifying the base or overlay directly.

### 💡 Real-World Relevance
- Image transformers streamline deployments across multiple environments by ensuring consistency in image versions.
- Patches make configuration management flexible and scalable, especially when teams collaborate across environments.
- Overlays simplify promotion pipelines (Dev → QA → Prod) with minimal manual edits.
- Components let teams toggle optional features like monitoring/ingress without duplicating configs.

### 💰 Cost-Saving Insight
- Overlays + components reduce YAML sprawl, saving engineering time and lowering operational risks.
- Image transformers ensure reliable rollouts, minimizing downtime and resource waste during deployments.
- Kustomize’s declarative patching avoids human errors, reducing rework and cost in large-scale environments.
