
Topics Covered today: Autoscaling

- **Horizontal Pod Autoscaler (HPA):**  
    Learned how HPA scales pods horizontally based on CPU/memory utilization metrics or custom metrics. Practiced creating HPA using `kubectl autoscale deployment` and verified scaling behavior with load generation.  
    Example:
    
    `kubectl autoscale deployment myapp --cpu-percent=50 --min=1 --max=10`
    
    Understood how HPA uses metrics-server data and adjusts replica count automatically.
    
- **Vertical Pod Autoscaler (VPA):**  
    Studied how VPA adjusts CPU and memory requests/limits for containers automatically. Learned about its three modes — _Off, Auto, Recreate_ — and how it works alongside (or separately from) HPA.  
    Key takeaway: VPA is ideal for optimizing pod resource requests over time, while HPA handles real-time scaling.
    
- **Cluster Autoscaler (CA):**  
    Explored how CA adds or removes nodes automatically when the cluster runs out of resources or becomes underutilized. Understood integration with cloud provider node groups (like GCP or AWS).
    
- **Core Concept Summary:**  
    HPA → Pod-level scaling (replica count)  
    VPA → Resource-level tuning (CPU/memory per pod)  
    CA → Node-level scaling (cluster capacity)
