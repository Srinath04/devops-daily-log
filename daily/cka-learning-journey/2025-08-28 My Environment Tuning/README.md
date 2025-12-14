
## 📌 Topics Covered

1. Shell environments: interactive vs. non-interactive and their impact on CLI tools like kubectl.

2. Output redirection in Linux (stdout, stderr, file descriptors, 2>&1).

3. Command-specific behavior of redirection (ls, cat, curl).

4. Customizing shell profiles (.bashrc, .zshrc) for better productivity and avoiding hangs.

5. Kubernetes resource quotas (kubectl describe resourcequota) and mapping them to system resources.

6. TLS in Kubernetes: connection with Secrets and usage in Gateways.

7. Checking container runtime in Kubernetes clusters.

8. Namespace management (kubectl config set-context).

9. Useful kubectl tricks (column printing, ingress class name lookup).

## 🔑 Key Learnings

- Shell Guarding: Adding conditions in .bashrc ensures that scripts/commands execute only in interactive shells—this prevents tools like kubectl from hanging in automation contexts.
- Redirection Mastery: Learned the difference between stdout (1) and stderr (2), and how to redirect or merge them (2> file, 2>&1). This is crucial for debugging logs in both Linux and Kubernetes pods.
- Command Behavior: Not all commands behave the same—ls may not generate stderr for existing files, while tools like curl can produce both output and error simultaneously.
- Silent Flags: curl -s suppresses progress meter and error messages, useful in scripting pipelines.
- Security in K8s: TLS is deeply tied to Kubernetes Secrets and is mandatory for Gateways in production-grade clusters.
- Practical Productivity: Managing namespaces, ingress classes, and runtime checks are daily ops skills for real clusters.


## 🛠 Commands & Configs Practiced
```
# Redirect only stderr
command 2> error.log  

# Merge stdout & stderr
command > all.log 2>&1  

# Curl silent with stderr only
curl -s http://localhost 2>&1  

# Switch namespace
kubectl config set-context --current --namespace=playground  

# Check resource quota
kubectl describe resourcequota myquota
```

## 🌍 Real-World Application

Troubleshooting & Debugging: Redirection knowledge directly applies to analyzing pod/container logs.

Cost Efficiency: Mapping CPU/memory allocations to actual hardware prevents over-provisioning cloud resources.

Security: Understanding TLS & Secrets ensures production traffic remains encrypted and compliant.
