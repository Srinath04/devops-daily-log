📅 2025-08-09

## 🔧 Activities

1. Tested DNS resolution for Kubernetes Services.
2. Explored CoreDNS verification methods using host inside pods and from Minikube VM.
3. Enabling kubectl autocompletion locally and its availability in other environment like exam 
4. Clarified shell startup file behavior in multi-shell (bash/zsh) environments, with focus on cross-environment portability.


## 🔍 Key Learnings
- Host not found in Minikube → Minimal OS image (Buildroot) ships without DNS utilities; must install (apk add bind-tools) or use an alternative like nslookup or a debug pod.
- CoreDNS tests → Best validated from inside a pod (kubectl exec) unless querying CoreDNS directly.
- Shell configs → For portability across shells and environments, put shared autocompletion setup in a common sourced file rather than editing .bashrc/.zshrc separately →Analyzing not implemented

📁 Related Files
- Not created — only shell snippet (~/.shell_common) if implemented.
