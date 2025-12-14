

Today’s progress was on **RBAC (Role-Based Access Control)** — understanding how **Roles**, **ClusterRoles**, **RoleBindings**, and **ClusterRoleBindings** work together to control access within a Kubernetes cluster. Also learned about **verbs** like `get`, `list`, `watch`, `create`, `update`, and how they define specific permissions for users or service accounts within namespaces or cluster-wide.

A subtle difference:   
`get` is for a **specific resource**, e.g. `kubectl get pod myapp`,  

`get` allows viewing **the details (metadata/status/spec)** of a resource.  
 But **not** all metadata — e.g., secrets data is Base64 encoded, but still considered readable via `get`.

while `list` is for **all resources of that type**, e.g. `kubectl get pods`.

`list` allows **enumerating all objects** in a namespace (or cluster if cluster-scoped).  
 But **cannot fetch individual details** unless user also have `get`.
