
CKA Daily Prep – ConfigMaps, Commands/Args, Secrets & TLS

**1. ConfigMap with Command & Args**

- ConfigMaps store key-value configuration data, mounted as env vars or volumes.
    
- When running a Pod:

    - `command:` in YAML overrides the image **ENTRYPOINT**.        
    - `args:` in YAML overrides the image **CMD**.
    - If not specified, container runs with image defaults.
        
- Example: Use `kubectl run` with `--command` flag, or define in Pod spec.
    
**2. Secrets**

- Kubernetes Secrets store sensitive data (passwords, tokens, certs).
- Can be:
    - **Opaque**: Generic base64 encoded data.
    - **TLS**: For certificates and keys (used in Ingress, TLS termination).

- Secrets can be mounted as env vars or files.    
- Always base64 encode before creating manually.
  
**3. TLS Secret**

- Special secret type: `kubernetes.io/tls`.
    
- Requires:  
    - `tls.crt` (public cert)
    - `tls.key` (private key)
        
- Usage: Attach to Ingress for HTTPS termination.
    
---
✅ **Focus Point for Exam**:

- Know how `command` vs `args` interact with ENTRYPOINT/CMD.
- Practice creating ConfigMaps and Secrets from both `kubectl` CLI and YAML.
- Be able to generate TLS secret and reference it in an Ingress.
