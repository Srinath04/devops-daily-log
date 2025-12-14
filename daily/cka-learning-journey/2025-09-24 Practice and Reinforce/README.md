
### Daily Documentation (CKA Exam Practice)

**Date:** 2025-09-24

#### Topics Practiced:

- Certificate Signing Request (CSR) handling in Kubernetes.
- Base64 encoding of certificate files for use in Kubernetes manifests.
- Role-based access validation using `kubectl auth can-i`.
- Understanding API naming conventions (Core API vs Named API).
- Kubernetes pod command and arguments behavior (entrypoint, args, and overrides).

#### Commands & Examples:

### Encode CSR file into a single-line base64 output

`cat common-user.csr | base64 | tr -d '\n'`
### Alternative way to encode CSR into single line

`cat common-user.csr | base64 -w 0`

## Check permissions for a user

```
kubectl auth can-i get pods --as=comm-user -n pg

kubectl auth can-i get pod click-app --as=comm-user -n pg

```
  
# Running a pod with custom command

kubectl run test-pod --image=nginx --command -- sleep 3600

#### Key Notes:

- Kubernetes `CertificateSigningRequest` objects require the CSR to be provided as a base64-encoded, single-line string.

- `base64 | tr -d '\n'` ensures newlines are stripped from the output.
    
- `base64 -w 0` directly outputs without line wrapping (simpler and preferred when available).
    
- `kubectl auth can-i` checks RBAC permissions for a given user in a namespace.
    
- Core APIs (e.g., v1) are lowercase, while named APIs (e.g., apps/v1, batch/v1) follow a `group/version` format.
    
- If no `command` or `args` are specified, pods run with the image’s default ENTRYPOINT and CMD.
    
---

**Practice Outcome:**

- Successfully generated single-line encoded CSR ready for Kubernetes CSR YAML.

- Validated RBAC permissions using `kubectl auth can-i`.
    
- Clarified API naming conventions for better YAML writing.
    
- Practiced pod command overrides for flexible testing.
