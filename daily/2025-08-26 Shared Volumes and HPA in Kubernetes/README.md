
## 🔧 Activity
Today’s focus was on Kubernetewqs storage and scaling fundamentals. 

1. Explored how multiple containers within the same pod share data using volumes and mounts 
2. How Horizontal Pod Autoscaler (HPA) scales workloads based on CPU usage and stabilization settings in HPA to prevent unnecessary scaling resource.
## 🔍 Key Learnings
### Shared Volumes in Pods

- A volume mounted to multiple containers in the same pod allows data exchange.
Example: One container writes logs, another container processes or ships them.  
Without volumes, containers are isolated with their own filesystem.

- Mounting Volumes
Volumes can be emptyDir, configMap, secret, or persistent storage.  
``mountPath`` inside a container defines where the data appears 

### Horizontal Pod Autoscaler (HPA) Basics
- HPA automatically adjusts the number of pod replicas based on CPU or memory utilization.
Example: Target CPU 50% → if actual CPU usage >50%, replicas increase  

### Stabilization Window in HPA
- Prevents frequent up/down scaling.  
Example: Even if CPU drops quickly, pods won’t be immediately scaled down; a waiting window ensures stability.

## 🖥️  Commands / Configs
### Shared Volume Yaml (Non Persistent)
```
apiVersion: v1
kind: Pod
metadata:
  name: shared storage pod
  namespace: ssp-playground
spec:
  volumes: 
  - name: shared-volume
    emptyDir: {}
  containers:
  - image: nginx:1-alpine
    name: ssp-1
    env:
      - name: NODE_NAME
        valueFrom:
          fieldRef:
            fieldPath: spec.nodeName
  - name: ssp-2
    image: busybox:1
    command:
    - "sh"
    - "-c"
    - "while true; do date >> /var/log/shared/date.log; sleep 10; done"
    volumeMounts:
      - name: shared-volume
        mountPath: /var/log/shared
  - name: ssp-3
    image: busybox:1     
    command:
    - "sh"
    - "-c"
    - "tail -f /var/log/shared/date.log"
    volumeMounts:
      - name: shared-volume        
        mountPath: /var/log/shared
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

### HPA with CPU Target and Stabilization:
```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-deployment-test
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-deployment
  minReplicas: 2
  maxReplicas: 10
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300   # wait 5 minutes before scaling down
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50

```

##3 Real-World Use Case
- Shared Volumes: Log shipping sidecars, where one container writes app logs and another container forwards them to ELK/CloudWatch.

- HPA with Stabilization: Online shopping app during flash sales → avoids rapid scale in/out by keeping enough pods alive during short demand spikes.
