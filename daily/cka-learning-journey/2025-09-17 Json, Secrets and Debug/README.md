
# 📘 Daily Documentation – 17 Sept 2025

## Activities

- **Mock Exam Practice**
    
    - Worked with **JSONPath queries** to extract specific fields from Kubernetes resources.
        
        - Example:
            
            `k get deployments.apps -o custom-columns='DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[0].image,READY REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace' --sort-by=.metadata.name'`
            
        - Validation checks
            
            `Unexpected token 'a', "apiVersion"... is not valid JSON`
            
            → Happens with validator mismatch
            Reminder: JSONPath works only with **JSON output** (`-o json`) 
            choose YAML validator or use tools like `yq` 
            
    - **Secret Volume Mounting**
        
        - Explored how to mount Kubernetes Secrets inside pods.
           
        - Example YAML:
            
            `apiVersion: v1 kind: Pod metadata:   name: secret-test-pod spec:   containers:   - name: app     image: nginx:1-alpine     volumeMounts:     - name: secret-vol       mountPath: "/etc/secret"       readOnly: true   volumes:   - name: secret-vol     secret:       secretName: my-secret`
            
        - Learned that secrets can be mounted as files inside the container or exposed as environment variables.
            
- **Downward API Practice**
    
    - Tested environment variables injected into pods using **fieldRef**.
        
    - Example:
        `env:   - name: NODE_NAME     valueFrom:       fieldRef:         fieldPath: spec.nodeName`
        
        - This dynamically gets the node name where the pod is scheduled.
        - To verify:
            `kubectl exec mc-pod-1 -- printenv | grep NODE_NAME`

- **General Mock Test Reflections**
  
1. **JSONPath** → custom columns and sorting.
2. **Secrets** → Two main usage patterns: `volumeMounts` (files) vs `env` (variables).
3. **Downward API** → Powerful for injecting pod metadata dynamically.
4.  **Mock Tests** → Help reveal small mistakes and debug quickly using `kubectl explain`
