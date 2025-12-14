
2025-10-05

labels & selectors, taints & tolerations, tmux scrolling, nodeSelector, nodeAffinity, and checking GCP billing account.

---

## 1) Labels and Selectors

**What is a label?**

- Key/value pair attached to Kubernetes objects (pods, nodes, services, etc.).
    
- Used to organize, select, and group objects.
    

**Common commands**

- Add a label: `kubectl label pod my-pod env=prod`
    
- List objects with a label: `kubectl get pods -l env=prod`
    
- Remove a label: `kubectl label pod my-pod env-`
    

**Selectors**

- **Equality-based**: `-l env=prod` or `-l 'tier!=frontend'`
    
- **Set-based**: `-l 'env in (prod,staging)'` or `-l 'env notin (dev)'`
    

**Examples**

- Select pods with app=api and tier=backend: `kubectl get pods -l app=api,tier=backend`
    
- Use in a Service spec: `selector: app: nginx`
    

**Tips / pitfalls**

- Labels are arbitrary strings but avoid using too generic keys.
    
- Removing or changing labels can immediately change Service/Deployment targeting.
    

---

## 2) Taints and Tolerations

**What is a taint?**

- A property on a **node** that repels pods unless those pods have a matching **toleration**.
    
- Syntax (imperative): `kubectl taint nodes <node> key=value:Effect`
    
- Effects: `NoSchedule`, `PreferNoSchedule`, `NoExecute`.
    

**Example**

- Add taint: `kubectl taint nodes node1 key=reserved:NoSchedule`
    
- Pod toleration (YAML):
    

```yaml
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "reserved"
    effect: "NoSchedule"
```

**Notes**

- `NoSchedule` blocks scheduling unless pod tolerates it.
    
- `NoExecute` will also evict existing pods that do not tolerate it.
    
- `PreferNoSchedule` is a soft preference.
    

**Common use-cases**

- Reserve nodes for special workloads (GPU nodes, high-memory, maintenance nodes).
    
- Isolate system-critical workloads from user workloads.
    

---

## 3) tmux — scroll / copy-mode

**Enter copy / scroll mode**

- Default key sequence: `Ctrl-b` then `[`
    
    - Once inside copy-mode: use arrow keys, PageUp/PageDown, or vi-style keys (if configured).
        
    - To copy: move cursor to start, press `Space`, move to end, press `Enter` — copied into tmux buffer.
        
- Exit copy-mode: `q` or `Enter`.
    

**Mouse scroll**

- Enable mouse mode to use mouse wheel: add to `~/.tmux.conf`: `set -g mouse on` then reload config: `tmux source-file ~/.tmux.conf`.
    

**Paste buffer**

- Paste last tmux buffer: `Ctrl-b` then `]`.
    
- List tmux buffers: `tmux list-buffers` and paste specific: `tmux paste-buffer -b <buffer-index>`.
    

**Tips**

- If terminal supports it, use `set -g mouse on` for quicker scroll.
    
- Use vi keys by enabling `setw -g mode-keys vi` in tmux config.
    

---

## 4) nodeSelector (pod spec)

**What is nodeSelector?**

- Simple key/value map used to schedule pods onto nodes that match the labels exactly.
    

**Example pod snippet**

```yaml
spec:
  nodeSelector:
    kubernetes.io/hostname: node-01
    workload: payload
```

**When to use**

- Quick, exact matches when you only need simple scheduling constraints.
    

**Limitations**

- It is exact-match only, no operators or multiple terms like nodeAffinity.
    

---

## 5) nodeAffinity

**What is nodeAffinity?**

- More expressive way to constrain pod scheduling based on node labels.
    
- Supports `requiredDuringSchedulingIgnoredDuringExecution` (hard) and `preferredDuringSchedulingIgnoredDuringExecution` (soft).
    

**Example — required (hard)**

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: "node"
          operator: In
          values:
          - worker3
```

**Example — preferred (soft)**

```yaml
affinity:
  nodeAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
    - weight: 1
      preference:
        matchExpressions:
        - key: "zone"
          operator: In
          values: ["zone-a", "zone-b"]
```

**When to prefer nodeAffinity**

- Need `In`, `NotIn`, `Exists` operators or multiple alternative terms.
    
- Want a weighted preference rather than a hard requirement.
  

**Tips**

- `nodeSelector` is syntactic sugar for a simple `nodeAffinity` rule; use affinity for complex rules.
    
---

## 6) gcloud — checked for billing account to use the initial credits to build practical projects in coming days

