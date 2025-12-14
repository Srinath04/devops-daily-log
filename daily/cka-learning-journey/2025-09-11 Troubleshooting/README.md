
## Key Learnings

- **Service Labels and URLs**: Learned to retrieve Kubernetes service labels using commands like kubectl get services --show-labels and kubectl describe service <service-name>. Understood how to construct service URLs based on service types (e.g., ClusterIP, NodePort) and extract them using kubectl get service -o json.
    
- **Minikube Troubleshooting**: Gained experience troubleshooting connection issues in Minikube, such as verifying NodePort, checking pod status with kubectl describe pod, and using minikube service to access applications. Learned to debug Minikube IP and firewall settings for connectivity.
    
- **Pod Communication**: Explored inter-container communication within a pod using a shared emptyDir volume to pass data (e.g., date output) between containers. Understood the importance of volume mounts and container configurations for CKA tasks.
    

## Commands/Configs

- **Retrieve Service Labels**:
    
    ```bash
    kubectl get services --show-labels
    kubectl describe service my-service
    kubectl get service my-service -o jsonpath='{.metadata.labels}'
    ```
    
- **Access Service URL**:
    
    ```bash
    kubectl get services
    minikube service my-service --url  # For Minikube NodePort
    kubectl get service my-service -o jsonpath='{.spec.clusterIP}:{.spec.ports[0].port}'
    ```
    
- **Minikube Troubleshooting**:
    
    ```bash
    minikube ip
    kubectl get pods -o wide
    kubectl describe service my-service
    kubectl logs pg-pod
    minikube service my-service --url
    ```
