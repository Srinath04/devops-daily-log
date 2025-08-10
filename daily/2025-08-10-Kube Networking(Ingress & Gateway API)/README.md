
## 🔧 Activity
- Studied Ingress and Gateway API concepts in Kubernetes.
- Compared differences between the two in terms of architecture and use cases.

## 🔍 Key Learnings
Ingress:

Manages HTTP/HTTPS routing for services inside a Kubernetes cluster.
Requires an Ingress Controller (e.g., NGINX, HAProxy, Traefik).
More mature but less flexible for advanced routing.

Gateway API:

A newer,robust, more expressive and extensible alternative to Ingress.
Supports multiple traffic types — including HTTP, gRPC, TCP, and UDP — depending on the controller implementation.
Allows role-based control (like infra provitioning team controls Gateways, app teams control Routes).
Still evolving, but gaining adoption in managed Kubernetes platforms.
